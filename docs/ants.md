# Compiling ANTS from ARM64

## Set up neurodocker

```bash
pip install neurodocker
```
{{ or }}
```bash
conda create -n neurodocker python pyyaml
conda activate neurodocker
python -m pip install neurodocker
neurodocker --help
```
{{ or }}
```bash
docker run --rm repronim/neurodocker:0.7.0 --help
```
{{ or }}
```bash
singularity run docker://repronim/neurodocker:0.7.0 --help
```

## Generating the ANTS Dockerfile

```bash
neurodocker generate docker \
    --pkg-manager apt \
    --base-image debian:buster-slim \
    --ants version=2.3.4 \
> ants-234.Dockerfile

docker build --tag ants:2.3.4 --file ants-234.Dockerfile .
```

## Take a look at the generated Dockerfile

```bash
cat ants-234.Dockerfile
```

## Build ANTS
    
    ```bash
    docker build --tag ants:2.3.4 --file ants-234.Dockerfile .
    ```
## Run ANTS

```bash
docker run --rm ants:2.3.4 antsRegistration --version
```

## Run ANTS with a mounted volume and a working directory

```bash
docker run --rm -v $PWD:/data -w /data ants:2.3.4 antsRegistration --version
```
## Run antsBrainExtraction.sh

```bash
docker run --rm -v $PWD:/data -w /data ants:2.3.4 antsBrainExtraction.sh -d 3 -a 101_aa -e brainWithSkullTemplate.nii.gz -m brainPrior.nii.gz -o anat_Stripped.nii