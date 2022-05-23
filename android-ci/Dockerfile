# see https://github.com/bitrise-io/android/blob/master/Dockerfile

FROM flat/ubuntu:20.04
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

# Just matched `app/build.gradle`
ENV ANDROID_COMPILE_SDK "32"
ENV ANDROID_BUILD_TOOLS "32.0.0"
# Version from https://developer.android.com/studio#cmdline-tools
ENV ANDROID_SDK_TOOLS "8512546"
ENV ANDROID_SDK_ROOT /opt/android-sdk-linux

ENV ANDROID_HOME /android-sdk-linux
ENV PATH="${PATH}:/android-sdk-linux/platform-tools/"

RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
    openjdk-11-jdk libc6 libstdc++6 libgcc1 libncurses5 libz1 net-tools wget unzip \
    ruby ruby-dev gcc g++ libffi-dev make \
  && rm -rf /var/lib/apt/lists/* \
  && update-alternatives --set javac /usr/lib/jvm/java-11-openjdk-amd64/bin/javac \
  && update-alternatives --set java /usr/lib/jvm/java-11-openjdk-amd64/bin/java

# Download Android Command line Tools into $ANDROID_SDK_ROOT & Install Android SDKs and other build packages
ENV PATH ${PATH}:${ANDROID_SDK_ROOT}/platform-tools:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin
RUN cd /opt \
  && wget -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS}_latest.zip -O android-commandline-tools.zip \
  && mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools \
  && unzip -q android-commandline-tools.zip -d /tmp/ \
  && mv /tmp/cmdline-tools/ ${ANDROID_SDK_ROOT}/cmdline-tools/latest \
  && rm android-commandline-tools.zip \
  && yes | sdkmanager --licenses \
  && sdkmanager --update --channel=0 \
  && touch /root/.android/repositories.cfg \
  && yes | sdkmanager \
    "emulator" \
    "platform-tools" \
    "platforms;android-${ANDROID_COMPILE_SDK}" \
    "build-tools;${ANDROID_BUILD_TOOLS}"

# Fastlane
ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
RUN gem install fastlane --no-document \
 && fastlane --version