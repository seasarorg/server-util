# Migrating from Subversion to Git

Subversion から Git リポジトリへ移行するためのユーティリティコマンドです。

## Example: S2Directory

作業ディレクトリを作成します。

```
$ mkdir s2directory.git
$ cd s2directory.git
```

移行用 Git リポジトリを初期化します。

```
$ git svn init \
--tags tags/s2directory/* \
--trunk trunk \
--ignore-paths='^trunk/www/' \
https://www.seasar.org/svn/sandbox/s2directory/
```

Subversion からコミット情報を取得して Git へ変換します。

```
$ git svn fetch
```

### migrate-svn-to-git-tags

タグの候補を表示します。GIT_COMMITTER_DATE は、タグを付けた日付を再現するためのものです。
git-svn-id の情報を使用するため、migrate-svn-to-git-filter より先に実行する必要があります。

```
$ migrate-svn-to-git-tags
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

### migrate-svn-to-git-filter

不要な情報を削除します。

* remove git-svn-id
* replace empty commit messages
* [branches only] remove empty commit

```
$ migrate-svn-to-git-filter
```

### migrate-svn-to-git-push USER_NAME REPOSITORY_NAME

GitHub へ push します。下記は seasarorg ユーザの test-s2directory3 リポジトリへ push する例です。

```
$ migrate-svn-to-git-push seasarorg test-s2directory3
```

