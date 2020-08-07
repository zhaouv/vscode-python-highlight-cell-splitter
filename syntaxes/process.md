# inject

do the following steps to the markdown tmLanguage file, and reject to source.python

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

+ `(?<=<string>)[^<]*\.markdown(?=</string>)` => `$0.python`

+ `START_MARK_1` => `^# |\G# `
+ `START_MARK_2` => `^# `
+ `END_MARK_1` => `$(?:\\n# )`

手动修改部分 fenced_code_block_unknown +
```xml
      <key>fenced_code_block_unknown</key>
      <dict>
        <key>begin</key>
        <string>(^# |\G# )(\s*)(`{3,}|~{3,})\s*(?=([^`~]*)?$)</string>
        <key>beginCaptures</key>
        <dict>
          <key>1</key>
          <dict>
            <key>name</key>
            <string>comment.punctuation.definition.markdown.python</string>
          </dict>
          <key>3</key>
          <dict>
            <key>name</key>
            <string>punctuation.definition.markdown.python</string>
          </dict>
          <key>4</key>
          <dict>
            <key>name</key>
            <string>fenced_code.block.language</string>
          </dict>
        </dict>
        <key>end</key>
        <string>(^# |\G# )(\2|\s{0,3})(\3)\s*$|(^|\G)(?=[^#\s]|#\s*%%|\s+\S)</string>
        <key>endCaptures</key>
        <dict>
          <key>1</key>
          <dict>
            <key>name</key>
            <string>comment.punctuation.definition.markdown.python</string>
          </dict>
          <key>3</key>
          <dict>
            <key>name</key>
            <string>punctuation.definition.markdown.python</string>
          </dict>
        </dict>
        <key>while</key>
        <string>(^# |\G# )(?!(\2|\s{0,3})(\3)\s*$)</string>
        <key>whileCaptures</key>
        <dict>
          <key>1</key>
          <dict>
            <key>name</key>
            <string>comment.punctuation.definition.markdown.python</string>
          </dict>
        </dict>
        <key>name</key>
        <string>markup.fenced_code.block.markdown.python</string>
      </dict>
```


# reinject

to make it possible to change the color of `^#`
``` 
old:
punctuation.definition.comment.python
comment.line.number-sign.python
source.python

before reinject:
markup.list.unnumbered.markdown.python
comment.markdown.python
source.python
```

# to test
## Aaaaa Bbbbb
`asd` a  
+ asd
+ as
  + sda

  + asd
+ [x] sda
1. asd
  1. asd
  1. 233
1. asd
[asda](abc.md)
![asda](abc.img)
aaaaaa aaaaaaaaaaa aaa aaa aaaaaaaa aaaa aaa
this should break the cell
```
a=1
b=2
```
asd