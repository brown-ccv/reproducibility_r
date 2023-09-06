To build the image on my laptop:
```
docker buildx build --platform linux/amd64,linux/arm64 --push -t compbiocore/reproducibility_r:20230906 .
```


To run it locally w docker
```
docker run --rm -p 8787:8787 -e USER=rstudio -e PASSWORD=yourpassword --volume ${PWD}:/home/rstudio compbiocore/reproducibility_r:20230906
```

To run it on oscar:
```
mkdir -p run var-lib-rstudio-server

printf 'provider=sqlite\ndirectory=/var/lib/rstudio-server\n' > database.conf

export SINGULARITY_BINDPATH="/gpfs/home/$USER,/gpfs/scratch/$USER"

singularity pull reproducibility-r-20230906.sif docker://compbiocore/reproducibility_r:20230906

export PASSWORD="mypasswordhere"

singularity exec --bind run:/run,var-lib-rstudio-server:/var/lib/rstudio-server,database.conf:/etc/rstudio/database.conf reproducibility-r-20230906.sif /usr/lib/rstudio-server/bin/rserver --auth-none=0 --auth-pam-helper-path=pam-helper --server-user=$(whoami)
```

then:
```
module load firefox/87.0
firefox
```
Point your browser to localhost:8787, enter your local user ID (e.g., oscar username) on the system as the username, and the custom password specified in the PASSWORD environment variable.