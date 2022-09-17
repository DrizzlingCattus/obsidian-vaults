
# word와 WORD의 차이 #anki 

Vim Help 문서를 읽을 때 word와 WORD를 구분할 줄 알아야 명령어가 어떻게 동작할지 명확하게 이해할 수 있다.

- word : letter, digit, underscore, 그 외 non-block 문자열 모음 각각이 word로 취급된다.  
- WORD : non-blank가 아닌 문자열의 모음. 즉 blank를 기준으로 WORD가 나뉜다. move 명령어 및 매크로 사용 시에 유용하다.

- ex) `hello-12 world-yo bro`
	- 7개의 word로 구성: hello, -, 12, world, -, yo, bro
	- 3개의 WORD로 구성: hello-12, world-yo, bro

```bash
:h word

							*word*
A word consists of a sequence of letters, digits and underscores, or a
sequence of other non-blank characters, separated with white space (spaces,
tabs, <EOL>).  This can be changed with the 'iskeyword' option.  An empty line
is also considered to be a word.
---
:h WORD

							*WORD*
A WORD consists of a sequence of non-blank characters, separated with white
space.  An empty line is also considered to be a WORD.

A sequence of folded lines is counted for one word of a single character.
"w" and "W", "e" and "E" move to the start/end of the first word or WORD after
a range of folded lines.  "b" and "B" move to the start of the first word or
WORD before the fold.

Special case: "cw" and "cW" are treated like "ce" and "cE" if the cursor is
on a non-blank.  This is because "cw" is interpreted as change-word, and a
word does not include the following white space.

Another special case: When using the "w" motion in combination with an
operator and the last word moved over is at the end of a line, the end of
that word becomes the end of the operated text, not the first word in the
next line.

The original Vi implementation of "e" is buggy.  For example, the "e" command
will stop on the first character of a line if the previous line was empty.
But when you use "2e" this does not happen.  In Vim "ee" and "2e" are the
same, which is more logical.  However, this causes a small incompatibility
between Vi and Vim.
```
