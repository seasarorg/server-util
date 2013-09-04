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
GIT_COMMITTER_DATE='1136764858' git tag -a 0.1 -m 'Tag S2Directory 0.1 release' 51b134c3cf5db7109faa02ac87a3204d937a9950
GIT_COMMITTER_DATE='1146332040' git tag -a 0.2 -m 'Tag S2Directory 0.2 release' d25651788d9d41bb373e37ac548125d1523f846d
GIT_COMMITTER_DATE='1146337792' git tag -a 0.2.1 -m 'Tag S2Directory 0.2.1 release' 17b7cd019f69c062a70cc116435912f073b5384c
GIT_COMMITTER_DATE='1146495006' git tag -a 0.2.2 -m 'Tag S2Directory 0.2.2 release' 1d85f56d7b4e5c8a59fd2d672f1e9718b8b123db
GIT_COMMITTER_DATE='1158898487' git tag -a 0.3 -m '' 4b4bbd7f2216a67379286308489a091ddced19c7
GIT_COMMITTER_DATE='1158904419' git tag -a 0.3.1 -m 'S2Directory 0.3.1 release' de6059369ea83bee85775a37b0bb07815c119a84
GIT_COMMITTER_DATE='1165812979' git tag -a 0.4 -m 'S2Directory 0.4 release' 7454bc0397e495f1bba484fbdd4be13ae7f39038
GIT_COMMITTER_DATE='1167527646' git tag -a 0.5 -m 'S2Directory 0.5 release' 0fee61a770d9d03a70f094c24e066b89d77a144b
GIT_COMMITTER_DATE='1184659354' git tag -a 0.5.1 -m 'S2Directory 0.5.1 release' 7562e1ed53240a6cf4a04920be1b4802d0ac3fdf
# parent commit not found in branches
GIT_COMMITTER_DATE='1198858530' git tag -a 0.6 -m 'S2Directory 0.6 release' 7072e42ecc1ebfb7a154b73325dbb083911cc2c8
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

```
$ migrate-svn-to-git-filter
```

### migrate-svn-to-git-push USER_NAME REPOSITORY_NAME

GitHub へ push します。下記は seasarorg ユーザの test-s2directory3 リポジトリへ push する例です。

```
$ migrate-svn-to-git-push seasarorg test-s2directory3
```

