FROM openjdk:8-jdk

LABEL maintainer="Peerapong Samarnpong<peerapong_sam@hotmail.com>"

ARG ANDROID_SDK_TOOLS="4333796"
ARG ANDROID_BUILD_TOOLS="28.0.3"
ARG ANDROID_COMPILE_SDK="28"
ARG GRADLE_VERSION="4.10.1"
ARG KOTLIN_VERSION="1.3.21"

ENV ANDROID_HOME "/opt/android-sdk"
ENV GRADLE_HOME="/opt/gradle"
ENV KOTLIN_HOME="/opt/kotlinc"
ENV PATH="$PATH:${GRADLE_HOME}/bin:${KOTLIN_HOME}/bin:${ANDROID_HOME}/emulator:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools"

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    wget \
    tar \
    unzip \
    lib32stdc++6 \
    lib32z1 \
  && rm -rf /var/lib/apt/lists/*

# Install gradle
RUN cd /opt\
  && wget --quiet --output-document=gradle.zip https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip \
  && unzip gradle.zip \
  && ls -d */ | sed 's/\/*$//g' | xargs -I{} mv {} gradle \
  && rm gradle.zip

# Install kotlin compiler
RUN cd /opt \
  && wget --quiet --output-document=kotlinc.zip https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip \
  && unzip kotlinc.zip \
  && rm kotlinc.zip

# Install android sdk
RUN mkdir -p $ANDROID_HOME \
  && cd $ANDROID_HOME \
  && wget --quiet --output-document=android-sdk.zip https://dl.google.com/android/repository/sdk-tools-linux-${ANDROID_SDK_TOOLS}.zip \
  && unzip -q android-sdk.zip \
  && rm android-sdk.zip

RUN mkdir ~/.android \
  && touch ~/.android/repositories.cfg \
  && mkdir -p $ANDROID_HOME/licenses \
  && echo -e "\n8933bad161af4178b1185d1a37fbf41ea5269c55\nd56f5187479451eabf01fb78af6dfcb131a6481e" > $ANDROID_HOME/licenses/android-sdk-license \
  && echo -e "\n84831b9409646a918e30573bab4c9c91346d8abd" > $ANDROID_HOME/licenses/android-sdk-preview-license \
  && echo y | ${ANDROID_HOME}/tools/bin/sdkmanager --licenses \
  && echo y | ${ANDROID_HOME}/tools/bin/sdkmanager --update
 
RUN echo y | sdkmanager "tools" \
    "platform-tools" \
    "build-tools;${ANDROID_BUILD_TOOLS}" \
    "platforms;android-28" \
    "platforms;android-27" \
    "extras;android;m2repository" \
    "extras;google;m2repository" \
    "extras;google;google_play_services"