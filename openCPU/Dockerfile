FROM opencpu/base:latest

RUN R -e 'install.packages("remotes", repos = "https://cloud.r-project.org")'
RUN R -e 'remotes::install_github("kwb-r/kwb.heatsine.opencpu@v0.1.0")'
