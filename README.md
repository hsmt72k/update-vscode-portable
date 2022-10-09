## VSCode ポータブル版のアップデート方法

### VSCode ポータブル版の問題

- ポータブル版の VSCode はインストーラ版と違って自動アップデートが使えない
- したがって、簡易なアップデートが可能なように以下の手順、スクリプトが GitHub コミュニティで提案されている

### スクリプト - Windows

`update.ps1`

``` powershell
 # Remove temp file from portable user data
 Remove-Item -Recurse -Force -Path "data/user-data" -Include @("Backups", "Cache", "CachedData", "GPUCache", "logs")
 
 # Download latest stable build
 curl.exe -L "https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-archive" -o stable.zip
 
 # Delete anything except user data, update script and downloaded zip file
 Get-ChildItem -Exclude @("data", "update.ps1", "stable.zip") | Remove-Item -Recurse -Force
 
 # Unzip it
 Expand-Archive -Path "stable.zip" -DestinationPath .
 
 # Delete downloaded package
 Remove-Item -Path "stable.zip"
 
 Pause
```

### update.ps1の実行

``` console
./update.ps1
```

- 上記のスクリプトを VSCode の実行ファイル（code.exe）がある場所に `update.ps1` というファイル名で保存する
- VSCode をアップデートしたい時はこのスクリプトを実行する
- なお、このスクリプトは data フォルダ以下のユーザー設定や拡張機能ファイルを除く、すべてのファイルを削除する
- なので、心配であれば事前に VSCode の入ったフォルダごとバックアップしておく
