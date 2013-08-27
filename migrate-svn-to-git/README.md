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

### migrate-svn-to-git-filter

不要な情報を削除します。

* remove git-svn-id
* replace empty commit messages
* [branches only] remove empty commit

```
$ migrate-svn-to-git-filter
```

### migrate-svn-to-git-tags

タグの候補を表示します。GIT_COMMITTER_DATE は、タグを付けた日付を再現するためのものです。

```
$ migrate-svn-to-git-tags
GIT_COMMITTER_DATE='1136766716' git tag -a 0.1 -m 'Tag S2Directory 0.1 release'
GIT_COMMITTER_DATE='1146332857' git tag -a 0.2 -m 'Tag S2Directory 0.2 release'
GIT_COMMITTER_DATE='1146363719' git tag -a 0.2.1 -m 'Tag S2Directory 0.2.1 release'
GIT_COMMITTER_DATE='1146495730' git tag -a 0.2.2 -m 'Tag S2Directory 0.2.2 release'
GIT_COMMITTER_DATE='1158903962' git tag -a 0.3 -m '<empty commit message>'
GIT_COMMITTER_DATE='1158904666' git tag -a 0.3.1 -m 'S2Directory 0.3.1 release'
GIT_COMMITTER_DATE='1165854783' git tag -a 0.4 -m 'S2Directory 0.4 release'
GIT_COMMITTER_DATE='1172931103' git tag -a 0.5 -m 'S2Directory 0.5 release'
GIT_COMMITTER_DATE='1184660603' git tag -a 0.5.1 -m 'S2Directory 0.5.1 release'
GIT_COMMITTER_DATE='1198858559' git tag -a 0.6 -m 'S2Directory 0.6 release'
GIT_COMMITTER_DATE='1205733258' git tag -a 0.6.1 -m 'S2Directory 0.6.1 release'
```

実際にタグを付与します。この時、必要に応じてメッセージやバージョンを修正することもできます。

```
$ GIT_COMMITTER_DATE='1136766716' git tag -a 0.1 -m 'Tag S2Directory 0.1 release'
$ GIT_COMMITTER_DATE='1146332857' git tag -a 0.2 -m 'Tag S2Directory 0.2 release'
...
```

### migrate-svn-to-git-push USER_NAME REPOSITORY_NAME

GitHub へ push します。下記は seasarorg ユーザの test-s2directory3 リポジトリへ push する例です。

```
$ migrate-svn-to-git-push seasarorg test-s2directory3
```

