# Railsセットアップメモ

## 参考サイト
[rbenv や Bundler を用いた Ruby環境構築](https://qiita.com/HayneRyo/items/d493a2b3cec2322f167c)

## セットアップ環境
$ cat /etc/debian_version
9.6

## セットアップについて
bashでのセットアップ手順を記載。
```
###################################################
# rbenv --version
# rbenv 1.1.1-39-g59785f6
#
###################################################
PROJECT_DIRECTORY=/path/to/project
BASHRC=~/.bashrc
# default package
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y git gcc make
sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev
sudo apt-get install -y libsqlite3-dev
sudo apt-get install -y nodejs

# rbenv check out.
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

# rbenv setup
grep -F -q -s 'export PATH=$HOME/.rbenv/bin:$PATH' ${BASHRC}
if [ $? -eq 1 ]; then
  cp ${BASHRC}{,`date +_%Y%m%d%H%M%S`}
  echo 'export PATH=$HOME/.rbenv/bin:$PATH' >> ${BASHRC}
fi
grep -F -q -s 'eval "$(rbenv init -)"' ${BASHRC}
if [ $? -eq 1 ]; then
  cp ${BASHRC}{,`date +_%Y%m%d%H%M%S`}
  echo 'eval "$(rbenv init -)"' >> ${BASHRC}
fi

# reload
. .bashrc

# ruby install
# rbenv install -l
rbenv install 2.5.3

# ruby setup
cd ${PROJECT}
rbenv local 2.5.3

# bundler setup
rbenv exec gem install bundler
rbenv rehash

rbenv exec bundle init
```

## Gemfiles
```
# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

gem 'rails', '~> 5.2'

```

# rails install
bundle install --path vendor/bundle
bundle exec rails new . -B --skip-turbolinks --skip-test

# Gemfileが最適な状態になるので、もう一度bundle install
bundle install --path vendor/bundle

# server start
bundle exec rails s