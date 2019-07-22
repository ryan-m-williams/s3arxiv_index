# s3arxiv_index
Script to generate index of Arxiv bulk tar files



Check tar file was downloaded intact
for i in `ls /mnt/filestore2/arXiv/arXiv_pdf_*tar`; do istar=$(tar --test-label -f $i 2>&1); if [[ ! -z "$istar" ]]; then echo $i $istar; fi; done;

For loop nest to iterate over each tar archive item and print the tar file and list values
for i in `ls /mnt/filestore2/arXiv/*.tar`; do tarlist=$(tar -tf $i); for j in $tarlist; do echo $i","$j >> tar.ls.20190719.csv; done; done;

Awk program to read tar list file and extract arxiv ID.
cat tar.ls.20190719.csv | grep "pdf$" | awk -F',' '{tarfile=substr($1, 23); arxivid=substr($2,6,length($2)-9)}{print tarfile"\t"$2"\t"arxivid}' > s3arxiv_dir.20190719.txt
