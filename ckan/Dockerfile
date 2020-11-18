FROM nikolayn/ckan-base:2.8

MAINTAINER Your Name Here <nikolay.nikolov@sintef.no>

# Set timezone
ARG TZ
RUN echo $TZ > /etc/timezone
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime

# Install CKAN DCAT extension
RUN pip install -e git+https://github.com/ckan/ckanext-dcat.git@v1.0.0#egg=ckanext-dcat && \
    pip install -r https://github.com/ckan/ckanext-dcat/raw/master/requirements.txt
RUN pip install ckanext-privatedatasets
RUN pip install ckanext-datarequests

COPY patches ${APP_DIR}/patches

RUN for d in $APP_DIR/patches/*; do \
        if [ -d $d ]; then \
            for f in `ls $d/*.patch | sort -g`; do \
                cd $SRC_DIR/`basename "$d"` && echo "$0: Applying patch $f to $SRC_DIR/`basename $d`"; patch -p1 < "$f" ; \
            done ; \
        fi ; \
    done
