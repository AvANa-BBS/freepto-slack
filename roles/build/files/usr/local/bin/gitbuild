#!/usr/bin/env bash
### This is similar to manualbuild.sh, but is oriented towards automatic build of a git remote
CACHE=/var/cache/build/repo
TMP=/var/tmp/build
WWW=/var/www/dev
REF=${1}
shift 1

set -e

if [[ ! -d "$CACHE" ]]; then
	mkdir -p "$(dirname "$CACHE")"
	git clone "https://github.com/avana-bbs/freepto-lb" "$CACHE"
	cd "$CACHE"
	git remote add vinc3nt "https://github.com/vinc3nt/freepto-lb"
	git remote add paskao "https://github.com/paskao/freepto-lb"
	git remote add boyska "https://github.com/boyska/freepto-lb"
	git remote update
	cd /
fi

mkdir -p "$TMP"
PARENT=$(mktemp -d "$TMP/git.XXXXXXX")
BASEDIR="$PARENT/${REF//\//+}"

cp -r "$CACHE" "$BASEDIR"

echo "Updating..."
git --git-dir "$BASEDIR/.git" --work-tree "$BASEDIR" remote update
echo "Checkout!"
git --git-dir "$BASEDIR/.git" --work-tree "$BASEDIR" checkout "$REF"
echo "Fetching submodules..."
(cd "$BASEDIR"; \
    git submodule init && \
    git submodule update)

ret=$?
if [[ $ret -ne 0 ]]; then
	echo "Error during git checkout: $?"
	exit $?
fi

echo "Ready to build"
manualbuild.sh $BASEDIR $*

# vim: set ft=sh:
