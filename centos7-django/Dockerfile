## ベースイメージ
FROM centos:centos7

## デフォルトシェルの指定
#SHELL ["/bin/bash", "-c"]

## 構築ディレクトを作成
WORKDIR /django

# 必要なパッケージをインストール
RUN yum -y update \
 && yum -y install \
    epel-release \
    gcc \
    git \
    vim \
    which \
    wget \
    make \
 && yum clean all

## ロケーション設定(日本語ロケールを設定)
RUN yum reinstall -y glibc-common \
 && yum clean all
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8 \
 && echo 'ZONE="Asia/Tokyo"' > /etc/sysconfig/clock \
 && ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
 && echo 'KEYMAP="jp106"' > /etc/vconsole.conf

## python3系をインストール
RUN yum install -y https://centos7.iuscommunity.org/ius-release.rpm
RUN yum install -y \
    python36u \
    python36u-devel \
    python36u-pip \
 && yum clean all \
 && ln -sf /usr/bin/python3.6 /usr/bin/python3 \
 && ln -sf /usr/bin/pip3.6 /usr/bin/pip3

## SQLite3.28.0 をインストール
WORKDIR /usr/local/src
RUN wget https://www.sqlite.org/2019/sqlite-autoconf-3280000.tar.gz \
 && tar xvfz sqlite-autoconf-3280000.tar.gz
WORKDIR ./sqlite-autoconf-3280000
RUN ./configure --prefix=/usr/local \
 && make \
 && make install
WORKDIR /usr/local/src
RUN rm -fr sqlite-autoconf-3280000* \
 && mv /usr/bin/sqlite3 /usr/bin/sqlite3_old \
 && ln -s /usr/local/bin/sqlite3 /usr/bin/sqlite3 \
 && echo "/usr/local/lib" > /etc/ld.so.conf.d/libSQLite.conf \
 && ldconfig -v

WORKDIR /django

## Python3 パッケージインストール
RUN pip3 install --upgrade pip
RUN pip3 install django

# Django プロジェクト起動
#RUN django-admin startproject helloworld

# helloworld プロジェクトへソースコード展開（Git Clone）
RUN git clone https://e2d6dc478be07d9bdc8e7acb42211a5576aebf6c:x-oauth-basic@github.com/totaloketani01/django-helloworld.git helloworld
