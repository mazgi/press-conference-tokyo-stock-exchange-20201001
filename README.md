# press-conference-tokyo-stock-exchange-20201001

## YouTube archives

- https://www.youtube.com/watch?v=Sokp32qOvyE
- https://www.youtube.com/watch?v=ACFLlMXhlWg

## ZShell Scripts

動作確認環境

```shellsession
❯ zsh --version
zsh 5.3 (x86_64-apple-darwin18.0)
```

動画開始時間に合わせてタイムライン生成

```shellsession
export BASE_SEC='0'; while read line
do
ts=$(echo $line | cut -d ' ' -f 1)
sec=$(($(TZ=UTC gdate --date "19700101 ${ts}" +%s) + ${BASE_SEC}))
datetime=$(TZ=UTC gdate --date "@$sec" +'%-k:%M:%S')
description="$(echo $line | cut -d ' ' -f 2-)"
echo "${datetime} ${description}"
```

質問をTSVに抽出

```shellsession
export YOUTUBE_URL='https://www.youtube.com/watch?v=Sokp32qOvyE'; echo "YouTube\tQuestioner" > questions.tsv && grep '(質問)' timeline.txt | while read line  
do
ts=$(echo $line | cut -d ' ' -f 1)
sec=$(($(TZ=UTC gdate --date "19700101 ${ts}" +%s)))
description=$(echo $line | cut -d ' ' -f 2- | sed -e 's/(質問)//g')
company=$(echo $description | cut -d ' ' -f 1)
person=$(echo $description | cut -d ' ' -f 2)
echo "=HYPERLINK(\"${YOUTUBE_URL}&t=${sec}s\",\"${ts}\")\t${description}\t${company}\t${person}"
done >> questions.tsv
```
