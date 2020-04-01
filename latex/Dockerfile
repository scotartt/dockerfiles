FROM debian:testing

ARG USER_NAME=latex
ARG USER_HOME=/home/latex
ARG USER_ID=1000
ARG USER_GECOS=LaTeX

RUN adduser \
  --home "$USER_HOME" \
  --uid $USER_ID \
  --gecos "$USER_GECOS" \
  --disabled-password \
  "$USER_NAME"

ARG WGET=wget
ARG GIT=git
ARG MAKE=make
ARG PANDOC=pandoc
ARG PYGMENTS=python-pygments
ARG FIG2DEV=fig2dev

RUN apt-get update && apt-get install -y \
  texlive-full \
  # some auxiliary tools
  "$WGET" \
  "$GIT" \
  "$MAKE" \
  # markup format conversion tool
  "$PANDOC" \
  # XFig utilities
  "$FIG2DEV" \
  # syntax highlighting package
  "$PYGMENTS" && \
  # Removing documentation packages *after* installing them is kind of hacky,
  # but it only adds some overhead while building the image.
  apt-get --purge remove -y .\*-doc$ && \
  apt-get clean -y

RUN wget --quiet -nc \
  --directory-prefix build/img \
  -nd https://github.com/jgm/pandoc/releases/download/2.9.2/pandoc-2.9.2-1-amd64.deb

RUN dpkg -i build/img/pandoc-2.9.2-1-amd64.deb

RUN apt-get update && apt-get install -y python3-pip

RUN pip3 -q install pandocfilters

RUN wget -nc --directory-prefix build/img \
  -nd https://www.tug.org/fonts/getnonfreefonts/install-getnonfreefonts

RUN texlua build/img/install-getnonfreefonts && getnonfreefonts --sys -a
