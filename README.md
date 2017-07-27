# nipype_afni_workflows

To run a docker environment to work in either:
```
cd afni_image
docker build -t afni_s -f Dockerfile_satra .
docker run -it -v nipype_afni_workflows/afni_data:/data -v nipype_afni_workflows/code/:/opt/tutorial -p 10104:10104 afni_s bash
```
and from inside the container run:
`jupyter notebook  --port=10104 --no-browser --allow-root`

or

```
cd afni_image
docker build -t afni_i -f Dockerfile_i .
docker run -v nipype_afni_workflows/afni_data:/root/CD -v nipype_afni_workflows/code:/root/code -p 10103:10103 -it afni_i /bin/bash
```
and from inside the container run:
`jupyter notebook --ip=0.0.0.0 --port=10103 --no-browser --allow-root`
