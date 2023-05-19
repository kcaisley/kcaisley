pandoc -s -f markdown -t html -o index.html index.md

rsync -r /Users/kcaisley/export/website/site/ kcaisley_kcaisley@ssh.phx.nearlyfreespeech.net:/home/public/
