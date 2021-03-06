# Migrating from Subversion to Git

Subversion から Git リポジトリへ移行するためのユーティリティコマンドです。

## 必要なコマンド

特筆すべき必要なコマンドです。

- git
- jq

## フォーマットの処理

フォーマットを処理します。

### migrate-svn-to-git

処理コマンドのオプションです。


```
Usage:
    migrate-svn-to-git [-a|-r package] [-p prefix] [-s suffix]

Options:
    -a Migrate all repositories
    -r Migrate the specified repository only. Available packages are:
        doma doma-gen doma-samples doma-tools mayaa mayaa-matatabi mayaa-struts2 mayaa-webwork2 mayaa-www s2directory s2directory-www
    -p prefix for new repository name
    -s suffix for new repository name
```

conf ディレクトリにあるフォーマットを個別に処理します。

```
$ ./migrate-svn-to-git -r s2director -p test- -s 5
```

## 個別スクリプト

作業ディレクトリを作成します。

```
$ mkdir s2directory.git
$ cd s2directory.git
```

移行用 Git リポジトリを初期化します。

```
$ git svn init \
--trunk=trunk \
--tags=tags/s2directory \
--branches=branches \
--ignore-paths='^trunk/www/' \
https://www.seasar.org/svn/sandbox/s2directory/
```

Subversion からコミット情報を取得して Git へ変換します。

```
$ git svn fetch --authors-file ~/git-svn-authors.txt
or
$ git svn fetch
```

### svn-tags-to-git-tags

タグの候補を表示します。GIT_COMMITTER_DATE は、タグを付けた日付を再現するためのものです。
git-svn-id の情報を使用するため、git-filter-remove-git-svn-id より先に実行する必要があります。

```
$ svn-tags-to-git-tags
GIT_COMMITTER_DATE='1136764858' git tag -a 0.1 -m 'Tag S2Directory 0.1 release' d1d331720535dc4d936f8cbc704c1beb95c9150a
GIT_COMMITTER_DATE='1146332040' git tag -a 0.2 -m 'Tag S2Directory 0.2 release' 2368b1168ae9bd7223dc568ed0b62b9c364bfc45
GIT_COMMITTER_DATE='1146337792' git tag -a 0.2.1 -m 'Tag S2Directory 0.2.1 release' 157f71c6736d6e700da3938387373251a0ef3e2b
GIT_COMMITTER_DATE='1146495006' git tag -a 0.2.2 -m 'Tag S2Directory 0.2.2 release' 1c0a8c54ff42eca2f9ecaeb8b2e73634cce4cd68
GIT_COMMITTER_DATE='1158898487' git tag -a 0.3 -m '' 9ae030160a86fb39aec5a60fc679364141fef4ed
GIT_COMMITTER_DATE='1158904419' git tag -a 0.3.1 -m 'S2Directory 0.3.1 release' fae91e71b8ef648f11f02ecb8a5d9f174e970ae7
GIT_COMMITTER_DATE='1165812979' git tag -a 0.4 -m 'S2Directory 0.4 release' 7f520656618d0a086e6ddf070ef68719a2cd7f77
GIT_COMMITTER_DATE='1167527646' git tag -a 0.5 -m 'S2Directory 0.5 release' e4fc97ae8592195ba94c6aa50f86feca6de15f65
GIT_COMMITTER_DATE='1184659354' git tag -a 0.5.1 -m 'S2Directory 0.5.1 release' da3b523a0b2c047b7ac4899e9b1dbcc532e0a596
GIT_COMMITTER_DATE='1213002755' git tag -a 0.6 -m 'S2Directory 0.6 release' 3469486766773ec3852b405e772f663c02259229
# parent commit not found in branches
GIT_COMMITTER_DATE='1205733081' git tag -a 0.6.1 -m 'S2Directory 0.6.1 release' 3d94b6d9087a6cb3beed431536c9c727feae9e58
```

実際にタグを付与します。この時、必要に応じてメッセージやバージョンを修正することもできます。

```
$ GIT_COMMITTER_DATE='1136766716' git tag -a 0.1 -m 'Tag S2Directory 0.1 release' b69b8783adad7ab538c69807d8b807d764480f61
$ GIT_COMMITTER_DATE='1146332857' git tag -a 0.2 -m 'Tag S2Directory 0.2 release' 58a4a94ae6e3af20696ca6b599708047687b270c
...
```

### [Option] git-filter-*

必要に応じて不要な情報を削除します。

* remove git-svn-id
* replace empty commit messages
* remove empty commit

```
$ git-filter-remove-git-svn-id
$ git-filter-replace-empty-commit-messages
$ git-filter-prune-empty
```

### git-push-*-to-github REPOSITORY_NAME

GitHub へ push します。下記は seasarorg ユーザの test-s2directory3 リポジトリへ push する例です。

```
$ export GITHUB_ORGS=seasarorg
$ git-push-master-to-github test-s2directory3
$ git-push-branches-to-github test-s2directory3 project-1.x,project-2.x
$ git-push-tags-to-github test-s2directory3
```

## License

Copyright 2013 The Seasar Foundation and the others

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
