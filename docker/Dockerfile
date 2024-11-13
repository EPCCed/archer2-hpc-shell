# FROM runiverse/base:latest
FROM rocker/rstudio:4.3.1

ENV MY_UNIVERSE='https://carpentries.r-universe.dev'

RUN apt update
RUN apt install -y libxml2-dev libfontconfig1-dev libxslt1-dev

RUN R -e 'install.packages(c("sandpaper", "varnish", "pegboard", "tinkr", "rmarkdown"), dep = TRUE, repos = c("https://carpentries.r-universe.dev/", getOption("repos")))'

COPY .Rprofile /home/rstudio/.Rprofile
COPY access.sh /etc/cont-init.d/99_access
