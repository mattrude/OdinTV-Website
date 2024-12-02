<h2>Download Instructions</h2>



<pre><code>
#!/bin/sh

URL="https://media.theodin.network"
URI="images/releases"

. /etc/os-release

FILE="${NAME}-${LIBREELEC_ARCH}-${VERSION}.img.gz"
CURRENT="`curl -Ls \"${fileURL}/Current.txt\"`"
fileURL="${URL}/${URI}/${VERSION_ID}/${LIBREELEC_DEVICE}"
fileCURRENT="`echo ${CURRENT} |awk '{ print $2 }'`"

cd /storage/.update/
if [ "${FILE}" != "${fileCURRENT}" ]; then
    echo "OdinTV update found"
    if [ -f /storage/.update/${fileCURRENT} ]; then
        echo " - Update already downloaded"
        rm -f `ls -t |grep -v ${fileCURRENT}`
        echo $CURRENT |sha256sum -cs -
        CHECKSUMOK=$?
    else
        echo " - Downloading update"
        rm -f *
        curl -Ls "${fileURL}/${fileCURRENT}" -o /storage/.update/${fileCURRENT}
        echo $CURRENT |sha256sum -cs -
        CHECKSUMOK=$?
    fi
    if [ ${CHECKSUMOK} == 0 ]; then
        echo " - Checksum is Good"
    else
        ls -lh /storage/.update/
        echo " - Checksum is Bad... Deleting bad file."
        rm -rf /storage/.update/*
    fi
fi
</code></pre>
