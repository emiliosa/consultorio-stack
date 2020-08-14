FROM openjdk:8-jdk

ENV ANDROID_HOME /usr/local/android-sdk-linux

RUN curl --fail -SL -o setup-nodejs https://deb.nodesource.com/setup_8.x &&\
    bash setup-nodejs &&\
    apt-get update -y -qq &&\
    apt-get install -y \
        build-essential \
        apt-transport-https \
        software-properties-common \
        ca-certificates \
        nodejs \
        unzip

RUN npm install -g cordova@latest
RUN npm install -g npm@latest
RUN npm cache clean --force

# ------------------------------------------------------
# --- Download Android SDK tools into $ANDROID_HOME
RUN touch ~/.repositories.cfg &&\
    mkdir /usr/local/android-sdk-linux &&\
    cd /tmp &&\
    curl --fail -SL -o android-sdk-linux.zip https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip &&\
    unzip -q android-sdk-linux.zip -d ${ANDROID_HOME} &&\
    rm android-sdk-linux.zip

ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools

# Accept licenses before installing components, no need to echo y for each component
# License is valid for all the standard components in versions installed from this file
# Non-standard components: MIPS system images, preview versions, GDK (Google Glass) and Android Google TV require separate licenses, not accepted there
RUN yes | sdkmanager --licenses

# Platform tools
RUN sdkmanager "emulator" "tools" "platform-tools"

# SDKs
# RUN yes | sdkmanager \
#     "platforms;android-28" \
#     "platforms;android-27" \
#     "platforms;android-26" \
#     "platforms;android-25" \
#     "platforms;android-24" \
#     "platforms;android-23" \
#     "platforms;android-22" \
#     "platforms;android-21" \
#     "platforms;android-19" \
#     "platforms;android-17" \
#     "platforms;android-15" \
#     "build-tools;28.0.3" \
#     "build-tools;28.0.2" \
#     "build-tools;28.0.1" \
#     "build-tools;28.0.0" \
#     "build-tools;27.0.3" \
#     "build-tools;27.0.2" \
#     "build-tools;27.0.1" \
#     "build-tools;27.0.0" \
#     "build-tools;26.0.2" \
#     "build-tools;26.0.1" \
#     "build-tools;25.0.3" \
#     "build-tools;24.0.3" \
#     "build-tools;23.0.3" \
#     "build-tools;22.0.1" \
#     "build-tools;21.1.2" \
#     "build-tools;19.1.0" \
#     "build-tools;17.0.0" \
#     "system-images;android-28;google_apis;x86" \
#     "system-images;android-26;google_apis;x86" \
#     "system-images;android-25;google_apis;armeabi-v7a" \
#     "system-images;android-24;default;armeabi-v7a" \
#     "system-images;android-22;default;armeabi-v7a" \
#     "system-images;android-19;default;armeabi-v7a" \
#     "extras;android;m2repository" \
#     "extras;google;m2repository" \
#     "extras;google;google_play_services" \
#     "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.2" \
#     "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.1" \
#     "add-ons;addon-google_apis-google-23" \
#     "add-ons;addon-google_apis-google-22" \
#     "add-ons;addon-google_apis-google-21"

# Gradle
RUN mkdir /usr/local/gradle &&\
    cd /tmp &&\
    curl --fail -SL -o gradle-bin.zip https://services.gradle.org/distributions/gradle-4.10.2-bin.zip &&\
    unzip -d /usr/local/gradle gradle-bin.zip

ENV PATH $PATH:/usr/local/gradle/gradle-4.10.2/bin

RUN apt-get clean
RUN mkdir -p /var/www/html

VOLUME ["/var/www/html"]

WORKDIR /var/www/html

CMD npm init
