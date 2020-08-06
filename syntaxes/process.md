# replace steps

https://github.com/microsoft/vscode-markdown-tm-grammar/blob/master/syntaxes/markdown.tmLanguage

specifically  
https://github.com/microsoft/vscode-markdown-tm-grammar/blob/4be9cb335581f3559166c319607dac9100103083/syntaxes/markdown.tmLanguage

(Notice that `\s` `[^]` do not contain `\n` in vscode)

+ `      <key>fenced_code([^u]|u(?!nknown))*([^\n]|\n(?!      </dict>))*^      </dict>` => empty
+ `(?<=#fenced_code_block)_(?!unknown)([^_]|\n|_(?!unknown))*` => empty
+ `      <key>frontMatter</key>` => empty
+ `^      <dict>([^d]|d(?!i))*?frontMatter[\s\S\n]*?^      </dict>` => empty


+ `\^\|\\G` => `START_MARK_1`
+ `(?<!\[)\^` => `START_MARK_2`
+ `\$\\n` => `END_MARK_1`

`\\[Ss]` 30的html那块正在处理 `<string>punctuation.definition.tag.end.html</string>`附近

`\\[Ss]` 39的html那团要细看下 `<string>meta.link.reference.def.markdown</string>`附近

`\\[Ss]` 50的bold那团要细看下 `<string>meta.other.valid-ampersand.markdown</string>`附近

+ `\.markdown</string>` => `.markdown.python</string>`

+ `START_MARK_1` => `^# |\G# `
+ `START_MARK_2` => `^# `
+ `END_MARK_1` => `$(?:\n# )`