Переименовываем .git/hooks/commit-msg.sample в .git/hooks/commit-msg
Этот хук отклонит коммит если сообщение больше 30 символов и если сообщения не соотверсивует требованиям: "[XX-name_module-XX-topic]"

Содержимое .git/hooks/commit-msg:

```bash
#!/bin/bash
count=$(cat "$1" | wc -m)
commitRegex='^(\[\d{2}-[a-zA-Z]+|merge)(-\d{2}-[a-zA-Z]+|merge)(\])'
if [[ $count -lt 1 ]] || [[ $count -gt 31 ]]	# Проверка на количество символов
then
	echo "The number of characters must be no more than 30"
	exit 1
else
	if ! grep -qE "$commitRegex" "$1"	# Проверка на корректность сообщения коммита. Должно быть, например, [04-script-01-bash]
	then
		echo "Aborting according commit message policy. Please specify [XX-name_module-XX-topic]."
		exit 1
	fi
fi
```
