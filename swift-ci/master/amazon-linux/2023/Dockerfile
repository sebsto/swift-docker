# Stage 1 : add ld.gold
# ld.gold is not part of Amazon Linux 2023, see https://github.com/amazonlinux/amazon-linux-2023/issues/330
FROM amazonlinux:2023 as GOLD
LABEL PURPOSE="This image is an intermediate image to build the ld.gold linker"

RUN dnf install -y \
   gmp-devel    \
   mpfr-devel   \
   texinfo      \
   bison        \
   git          \
   gcc-c++

RUN mkdir ld.gold \
&& cd ld.gold \
&& git clone --depth 1 git://sourceware.org/git/binutils-gdb.git binutils \
&& mkdir build && cd build \
&& ../binutils/configure --enable-gold --enable-plugins --disable-werror \
&& make all-gold \
&& cd gold \
&& make all-am \
&& cd .. \
&& cp gold/ld-new /usr/bin/ld.gold \
&& /usr/bin/ld.gold -v

# Stage 2: prepare the Swift build instance
FROM amazonlinux:2023
LABEL PURPOSE="This image is configured to build Swift for the version of Amazon Linux listed above"

COPY --from=GOLD /usr/bin/ld.gold /usr/bin

# Install build dependencies
RUN dnf -y install \
  clang            \
  cmake            \
  curl-devel       \
  gcc-c++          \
  git              \
  glibc-static     \
  libbsd-devel     \
  libedit-devel    \
  libicu-devel     \
  libuuid-devel    \
  libxml2-devel    \
  ncurses-devel    \
  ninja-build      \
  python3-pexpect  \
  pkgconfig        \
  procps-ng        \
  python           \
  python3-devel    \
  python3-six      \
  python3-psutil   \
  python3-pkgconfig\
  rsync            \
  sqlite-devel     \
  swig             \
  tzdata           \
  unzip            \
  uuid-devel       \
  wget             \
  which            \
  zip              \
  tar              \
  diffutils

  ## TODO add installation of Swift 5.8 to build Swift >= 5.9