FROM peerapongsam/android-sdk:latest

LABEL maintainer="Peerapong Samarnpong<peerapong_sam@hotmail.com>"

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    build-essential \
    ruby \
    ruby-dev \
    vim-common \
    libgmp-dev \
  && rm -rf /var/lib/apt/lists/*

# install FastLane
COPY Gemfile.lock .
COPY Gemfile .

RUN gem install bundle && bundle install