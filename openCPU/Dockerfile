FROM opencpu/base:v2.1.4

RUN R -e 'install.packages("remotes", repos = "https://cloud.r-project.org")'
RUN R -e 'remotes::install_github("KWB-R/kwb.qmra@dev")'