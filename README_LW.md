# VampNet

I recommend you read the README.md (from vampnet) first. However, don't create the conda environmnt as described there, since everything will be in the Docker. You also don't need to install vampnet since that is also in the docker. Vampnet installs as "editable" from the local directory. (Note: you will need to download the models (weights) as it says in the vampnet README)

To build the docker:

```
docker image build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) --file MyDockerfile.dev --tag lonce:vampnet .
```

 You can download the Descript codecs if you want and then specify the local path in the vampnet config files. For example, to download the 16kHz Descript codec to store it locally in vamnet/models  :
```wget https://github.com/descriptinc/descript-audio-codec/releases/download/0.0.5/weights_16khz.pth .```  (see blah/descript-audio-codec/dac/utils/__init__.py for other options)   

To run the docker:
```
docker run --ipc=host --gpus all -it -v $(pwd):/vampnet  -v /scratch:/scratch --rm lonce:vampnet
```
The first -v flag puts you in your vampnet director for running the code. The second one I use for storing my data. The last argument is the tag and name you gave to the docker when you built it. 

Then, inside the docker you can train:
```
python scripts/exp/train.py --args.load conf/myvampnet2.yml --save_path /scratch/lonce/ckpttmp2 2> /scratch/lonce/ckpttmp2/stderr.txt
```


