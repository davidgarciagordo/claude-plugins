# Statusline con badges (caveman + token-economy)

Prompt copy-paste para Claude Code: monta una statusline con directorio · rama git (con `*` si hay cambios) · modelo/estilo · % de contexto (aviso COMPACTA en rojo ≥78%) · límites 5h/7d con tiempo de reset · badges `[CAVEMAN]` y `[TOKEN-ECON]` si usas esos plugins.

Pégale esto a Claude Code tal cual:

---

Configúrame una statusline personalizada en Claude Code. Crea el fichero `~/.claude/statusline-command.sh` con este contenido exacto y hazlo funcionar:

```bash
#!/usr/bin/env bash
# Claude Code status line — dir + git + modelo/estilo + contexto(+compact) + límites(%+reset) + badges
input=$(cat)
j() { printf '%s' "$input" | jq -r "$1 // empty" 2>/dev/null; }

cwd=$(j '.workspace.current_dir'); [ -z "$cwd" ] && cwd=$(j '.cwd')
model=$(j ".model.display_name"); model="${model%% (*}"
used=$(j '.context_window.used_percentage')
effort=$(j '.effort.level'); [ -z "$effort" ] && effort=$(j '.effort_level')
out_style=$(j '.output_style.name')

home="$HOME"; short_dir="${cwd/#$home/~}"

color_for() {
  if [ "$1" -ge 80 ]; then printf '\033[0;31m'; elif [ "$1" -ge 50 ]; then printf '\033[0;33m'; else printf '\033[0;32m'; fi
}

# Git branch + dirty
git_branch=""
if git -C "$cwd" --no-optional-locks rev-parse --is-inside-work-tree >/dev/null 2>&1; then
  branch=$(git -C "$cwd" --no-optional-locks symbolic-ref --short HEAD 2>/dev/null || git -C "$cwd" --no-optional-locks rev-parse --short HEAD 2>/dev/null)
  if [ -n "$branch" ]; then
    dirty=""
    if ! git -C "$cwd" --no-optional-locks diff --quiet 2>/dev/null || ! git -C "$cwd" --no-optional-locks diff --cached --quiet 2>/dev/null; then
      dirty="\033[0;33m*\033[0;36m"
    fi
    git_branch=" \033[0;36m(${branch}${dirty})\033[0m"
  fi
fi

# Contexto + recomendación de compactar
ctx_part=""
if [ -n "$used" ]; then
  ui=$(printf '%.0f' "$used"); c=$(color_for "$ui")
  ctx_part=" ${c}ctx:${ui}%\033[0m"
  if [ "$ui" -ge 78 ]; then ctx_part="${ctx_part} \033[0;31mCOMPACTA\033[0m"
  elif [ "$ui" -ge 60 ]; then ctx_part="${ctx_part} \033[0;33m~compact\033[0m"; fi
fi

# Modelo + effort/estilo
model_part=""
if [ -n "$model" ]; then
  lab="$model"; [ -n "$effort" ] && lab="$lab/$effort"; [ -z "$effort" ] && [ -n "$out_style" ] && lab="$lab/$out_style"
  model_part=" \033[0;35m[$lab]\033[0m"
fi

# Límites 5h / 7d — % + tiempo hasta reset
fmt_reset() {
  local v="$1" ep; [ -z "$v" ] && return
  case "$v" in
    *[!0-9]*) ep=$(date -j -f "%Y-%m-%dT%H:%M:%S" "${v%%.*}" +%s 2>/dev/null || date -d "$v" +%s 2>/dev/null) ;;
    *) ep="$v" ;;
  esac
  [ -z "$ep" ] && return
  local now diff; now=$(date +%s); diff=$(( ep - now )); [ "$diff" -lt 0 ] && diff=0
  if [ "$diff" -ge 86400 ]; then printf 'r%dd%dh' $(( diff/86400 )) $(( (diff%86400)/3600 ))
  else printf 'r%dh%02dm' $(( diff/3600 )) $(( (diff%3600)/60 )); fi
}
limit_str() {
  local pct node="$1"
  pct=$(j "$node.used_percentage")
  [ -z "$pct" ] && return
  local pi; pi=$(printf '%.0f' "$pct"); local c; c=$(color_for "$pi")
  local reset; reset=$(j "$node.resets_at"); [ -z "$reset" ] && reset=$(j "$node.reset_at"); [ -z "$reset" ] && reset=$(j "$node.remaining")
  local r; r=$(fmt_reset "$reset")
  local warn=""; [ "$pi" -ge 90 ] && warn="!"
  printf ' %b%s:%s%%%s\033[0m%s' "$c" "$2" "$pi" "$warn" "${r:+ \033[0;90m$r\033[0m}"
}
rate_part="$(limit_str '.rate_limits.five_hour' '5h')$(limit_str '.rate_limits.seven_day' '7d')"

# Badge caveman — solo si usa el plugin caveman (flag file). Se omite solo si no existe.
cave_badge=""
cave_flag="${CLAUDE_CONFIG_DIR:-$HOME/.claude}/.caveman-active"
if [ -f "$cave_flag" ] && [ ! -L "$cave_flag" ]; then
  cm=$(head -c 64 "$cave_flag" 2>/dev/null | tr -d '\n\r' | tr '[:upper:]' '[:lower:]')
  cm=$(printf '%s' "$cm" | tr -cd 'a-z0-9-')
  case "$cm" in
    off|"") ;;
    lite|full|ultra|wenyan-lite|wenyan|wenyan-full|wenyan-ultra|commit|review|compress)
      if [ "$cm" = "full" ]; then cave_badge=" \033[38;5;172m[CAVEMAN]\033[0m"
      else cave_badge=" \033[38;5;172m[CAVEMAN:$(printf '%s' "$cm" | tr '[:lower:]' '[:upper:]')]\033[0m"; fi
      ;;
  esac
fi

# Badge token-economy — output style activo (stdin o persistido en settings.json)
te_badge=""
persisted_style=$(jq -r '.outputStyle // empty' "${CLAUDE_CONFIG_DIR:-$HOME/.claude}/settings.json" 2>/dev/null)
case "$out_style$persisted_style" in
  *token-economy*) te_badge=" \033[0;32m[TOKEN-ECON]\033[0m" ;;
esac

printf "\033[0;34m%s\033[0m%b%b%b%b%b%b" "$short_dir" "$git_branch" "$model_part" "$ctx_part" "$rate_part" "$cave_badge" "$te_badge"
```

Después:
1. Dale permisos de ejecución: `chmod +x ~/.claude/statusline-command.sh`.
2. Añade a mi `~/.claude/settings.json` (sin borrar lo que ya haya):
   ```json
   "statusLine": { "type": "command", "command": "bash ~/.claude/statusline-command.sh" }
   ```
3. Verifica que `jq` está instalado (`which jq`; si no, instálalo con brew/apt).
4. Prueba el script con un JSON de muestra por stdin y enséñame el resultado:
   ```bash
   echo '{"cwd":"'$PWD'","output_style":{"name":"default"},"model":{"display_name":"Sonnet"}}' | bash ~/.claude/statusline-command.sh
   ```

Notas: los badges `[CAVEMAN]` y `[TOKEN-ECON]` solo aparecen si tengo instalados esos plugins (caveman escribe `.caveman-active`, token-economy se activa con `"outputStyle": "token-economy:frugal"` en settings.json) — si no los uso, la barra funciona igual y simplemente no salen. La barra muestra: directorio · rama git (con `*` si hay cambios) · modelo/estilo · % de contexto (avisa COMPACTA en rojo ≥78%) · límites 5h/7d con tiempo de reset. En Linux el `date -j` de macOS falla silenciosamente y usa el fallback `date -d`, funciona igual.
