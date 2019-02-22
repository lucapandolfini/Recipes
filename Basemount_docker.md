## Install Docker Container

Install docker image for Basemount

```bash
mkdir ~/bin/
cd ~/bin

git clone https://github.com/muccg/docker-basemount.git
cd docker-basemount
```

Change `ENTRYPOINT ["basemount"]` to `ENTRYPOINT ["bash"]` in the `Dockerfile` and build containter:

```bash
docker build --tag=basemount .
```

## Run BaseMount

Mount Basespace filesystem:

```bash
docker run -it --privileged=true -v /home/lp471:/root basemount
basemount bm
```

Copy run files

```bash
##In container (root@7aba3edd1ddc):
cp /data/bm/Projects/HSXX/Samples/*/Files/*.fastq.gz ~/DATA/some_dir
```

After usage, unmount:

```bash
##In container (root@7aba3edd1ddc):
basemount --unmount /data/bm
```
