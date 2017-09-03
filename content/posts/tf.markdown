# first run with build-in model
### test installization

### transform data into TFFomart
use given data 
[ref](https://github.com/tensorflow/models/blob/master/object_detection/g3doc/preparing_inputs.md)
use custom data 
[ref](https://github.com/tensorflow/models/blob/master/object_detection/g3doc/using_your_own_dataset.md)

label map
rgb image + list of bounding box 


### prepare pipeline 
```object_detection/protos/pipeline.proto```
```object_detection/samples/configs/faster_rcnn_resnet101_pets.config```

### run locally 
```sh
python object_detection/train.py \
    --logtostderr \
    --pipeline_config_path="/mnt/d/Dropbox (Personal)/script/tf_related/object_detect_data/fi.configure"  \
    --train_dir="/mnt/d/Dropbox (Personal)/script/tf_related/object_detect_data/"
```

# model runtime routine 


### start with train.py
in trian.py 
handle cli arguemnet
handle pipeline argument 
fire ```trainer.train``` in trainer.py with argument 

### train in ```trainer.py```
in ```tarin()```

[0] dection_model & data_augmentation_option
(inside graph)
[1] get deploy_config
[2] get global_step
[3] get init_queue (prepross input?)

[4] gather summary 
what is clones?

[5] update_ops? 
[6] get traininig_optmizer, determine sync_optimizer or not

[7] create init_fn from given checkpoint
[8] get total_loss?

[9] optionaly freeze some layers / clip graidents
[10] gradients updates(?)

[11] add summaries
[12] save checkpoint regularly 




# complex version of linear regression 


# slim.model_deploy
```python
  g = tf.Graph()

  # Set up DeploymentConfig
  config = model_deploy.DeploymentConfig(num_clones=2, clone_on_cpu=True)

  # Create the global step on the device storing the variables.
  with tf.device(config.variables_device()):
    global_step = slim.create_global_step()

  # Define the inputs
  with tf.device(config.inputs_device()):
    images, labels = LoadData(...)
    inputs_queue = slim.data.prefetch_queue((images, labels))

  # Define the optimizer.
  with tf.device(config.optimizer_device()):
    optimizer = tf.train.MomentumOptimizer(FLAGS.learning_rate, FLAGS.momentum)

  # Define the model including the loss.
  def model_fn(inputs_queue):
    images, labels = inputs_queue.dequeue()
    predictions = CreateNetwork(images)
    slim.losses.log_loss(predictions, labels)

  model_dp = model_deploy.deploy(config, model_fn, [inputs_queue],
                                 optimizer=optimizer)

  # Run training.
  slim.learning.train(model_dp.train_op, my_log_dir,
                      summary_op=model_dp.summary_op)
```

# input data 

for input part, parallel reader is considered

```create_input_dict_fn``` is a partialed function of input_reader_builder.build, which 

```python
_, string_tensor = parallel_reader.parallel_read(
        config.input_path,
        reader_class=tf.TFRecordReader,
        num_epochs=(input_reader_config.num_epochs if input_reader_config.num_epochs else None),
        num_readers=input_reader_config.num_readers,
        shuffle=input_reader_config.shuffle,
        dtypes=[tf.string, tf.string],
        capacity=input_reader_config.queue_capacity,
        min_after_dequeue=input_reader_config.min_after_dequeue) # return a ParallelReader??

return tf_example_decoder.TfExampleDecoder().decode(string_tensor)
```

read from csv 
https://stackoverflow.com/questions/33808368/how-do-i-change-the-dtype-in-tensorflow-for-a-csv-file

http://honggang.io/2016/08/19/tensorflow-data-reading/


# faster_rcnn_meta_arch
major functions
```
preprocess: scaling, shifting, reshaping

predict: porduce RAW prediction tensors

postprocess: convert raw tensors to final detections

loss: compute the scalar loss tensor with respect to the groudtruth

restore:Load a checkpoint into the Tensorflow graph
```

# faster_rcnn_extractor
functions to define:
 ```python
preprocess

_extract_proposal_features

_extract_box_classifier_features

restore_from_classification_checkpoint_fn
 ```



# create a new feature extractor 
### 1 create a new feature extractor to be used in SSD or Faster R-CNN

define FasterRCNNFeatureExtractor 
pass it to FasterRCNNMetaArch

See object_detection/meta_architectures/faster_rcnn_meta_arch.py for their definitions 
See object_detection/models/faster_rcnn_resnet_v1_feature_extractor.py for example 

### 2 register the new model for configuration

### 3 fininally 
	test locally before training in the cloud 
	do a sweep of learning rates to figure the best learnining rate
	may need to disable batchnorm training 







