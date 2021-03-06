# マイクロサービスのイメージをACRにプッシュ

## やりたいこと
- Kubernetesでイメージをダウンロード(pull)して実行するために
- ビルドしたSpringBootアプリのdockerイメージをACRにプッシュしておく

## ビルドしてACRにプッシュを行うスクリプト
- 環境ごとに your-host の部分、your-username の部分を書き換える

```push-acr.sh```
```bash
#!/bin/bash

ACR_HOST=your-host.azurecr.io
ACR_USER=your-username

if [ "x$TAG" = "x" ]; then
    TAG=latest
fi

docker system prune -f
docker build -t echo-service .
echo $ACR_PASSWORD | docker login $ACR_HOST -u "$ACR_USER" --password-stdin
echo $ACR_HOST
docker tag shoprun-session "${ACR_HOST}/echo-service:${TAG}"
docker push "${ACR_HOST}/echo-service:${TAG}"
```

## 実行
```
ACR_PASSWORD="Azureポータルからパスワードをコピー" TAG=タグ ./push-acr.sh
```
