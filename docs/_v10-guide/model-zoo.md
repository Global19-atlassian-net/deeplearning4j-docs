---
title: Deeplearning4j Model Zoo
short_title: Model Zoo
description: Prebuilt model architectures and weights for out-of-the-box application.
category: Get Started
weight: 3
---

# Deeplearning4j Model Zoo

Deeplearning4j has native model zoo that can be accessed and instantiated directly from DL4J. The model zoo also includes pretrained weights for different datasets that are downloaded automatically and checked for integrity. 🚀

If you want to use the new model zoo, you will need to add it as a dependency. A Maven POM would add the following:

```
<dependency>
    <groupId>org.deeplearning4j</groupId>
    <artifactId>deeplearning4j-zoo</artifactId>
    <version>{{ page.version }}</version>
</dependency>
```

## Getting Started

Once you've successfully added the zoo dependency to your project, you can start to import and use models. Each model extends the `ZooModel` abstract class and uses the `InstantiableModel` interface. These classes provide methods that help you initialize either an empty, fresh network or a pretrained network.

### Initializing Fresh Networks

You can instantly instantiate a model from the zoo using the `.init()` method. For example, if you want to instantiate a fresh, untrained network of AlexNet you can use the following code:

```
import org.deeplearning4j.zoo.model.AlexNet
import org.deeplearning4j.zoo.*;

...

int numberOfClassesInYourData = 1000;
int randomSeed = 123;
int iterations = 1; // this is almost always 1

ZooModel zooModel = new AlexNet(numberOfClassesInYourData, randomSeed, iterations);
Model net = zooModel.init();
```

If you want to tune parameters or change the optimization algorithm, you can obtain a reference to the underlying network configuration:

```
ZooModel zooModel = new AlexNet(numberOfClassesInYourData, randomSeed, iterations);
MultiLayerConfiguration net = zooModel.conf();
```

### Initializing Pretrained Weights

Some models have pretrained weights available, and a small number of models are pretrained across different datasets. `PretrainedType` is an enumerator that outlines different weight types, which includes `IMAGENET`, `MNIST`, `CIFAR10`, and `VGGFACE`.

For example, you can initialize a VGG-16 model with ImageNet weights like so:

```
import org.deeplearning4j.zoo.model.VGG16;
import org.deeplearning4j.zoo.*;

...

ZooModel zooModel = new VGG16();
Model net = zooModel.initPretrained(PretrainedType.IMAGENET);
```

And initialize another VGG16 model with weights trained on VGGFace:

```
ZooModel zooModel = new VGG16();
Model net = zooModel.initPretrained(PretrainedType.VGGFACE);
```

If you're not sure whether a model contains pretrained weights, you can use the `.pretrainedAvailable()` method which returns a boolean. Simply pass a `PretrainedType` enum to this method, which returns true if weights are available.

Note that for convolutional models, input shape information follows the NCHW convention. So if a model's input shape default is `new int[]{3, 224, 224}`, this means the model has 3 channels and height/width of 224.



## What's in the Zoo?

The model zoo comes with well-known image recognition configurations in the deep learning community. The zoo also includes an LSTM for text generation, and a simple CNN for general image recognition.

You can find a complete list of models using this [deeplearning4j-zoo Github link](https://github.com/deeplearning4j/deeplearning4j/tree/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model).

This includes ImageNet models such as VGG-16, ResNet-50, AlexNet, Inception-ResNet-v1, LeNet, and more. 

* [AlexNet](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/AlexNet.java)	
* [Darknet19](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/Darknet19.java)	
* [FaceNetNN4Small2](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/FaceNetNN4Small2.java)	
* [InceptionResNetV1](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/InceptionResNetV1.java)	
* [LeNet](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/LeNet.java)
* [ResNet50](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/ResNet50.java)
* [SimpleCNN](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/SimpleCNN.java)
* [TextGenerationLSTM](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/TextGenerationLSTM.java)
* [TinyYOLO](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/TinyYOLO.java)
* [VGG16](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/VGG16.java)	
* [VGG19](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-zoo/src/main/java/org/deeplearning4j/zoo/model/VGG19.java)

## Advanced Usage

The zoo comes with a couple additional features if you're looking to use the models for different use cases.

### Model Selector

The `ModelSelector` allows you to select multiple models at once. This method was created for testing multiple models at once.

When you use the selector, it returns a `Map<ZooType, ZooModel>` collection. Passing `ZooType.RESNET50` to `ModelSelector.select(ZooType.RESNET50)` would return a collection of type `HashMap<ZooType.RESNET50, new ResNet50()>`. However, if you wanted to initialize multiple models for a specific dataset, you can pass appropriate params to the selector like `ModelSelector.select(ZooType.RESNET50, numLabels, seed, iterations)`.

Let's say you wanted to benchmark all of the models that had pretrained weights available for ImageNet. The following code allows you to select all of the convolutional models, check if weights are available, and then execute your own code:

```
import org.deeplearning4j.zoo.*;
...

// select all convolutional models
Map<ZooType, ZooModel> models = ModelSelector.select(ZooType.CNN);

for (Map.Entry<ZooType, ZooModel> entry : models.entrySet()) {
    ZooModel zooModel = entry.getValue();

    if(zooModel.pretrainedAvailable(PretrainedType.IMAGENET)) {
        Model net = zooModel.initPretrained(PretrainedType.IMAGENET);

        // do what you want with pretrained model
    }

    // clean up for current model (helps prevent OOMs)
    Nd4j.getWorkspaceManager().destroyAllWorkspacesForCurrentThread();
    System.gc();
    Thread.sleep(1000);
}
```

Alternatively, you can select specific models by doing:

```
ModelSelector.select(ZooType.RESNET50, ZooType.VGG16, ZooType,VGG19);
```

### Changing Inputs

Aside from passing certain configuration information to the constructor of a zoo model, you can also change its input shape using `.setInputShape()`. NOTE: this applies to fresh configurations only, and will not affect pretrained models:

```
ZooModel zooModel = new ResNet50(10, 123, 1);
zooModel.setInputShape(new int[]{3,28,28});
```

### Transfer Learning

Pretrained models are perfect for transfer learning! You can read more about transfer learning using DL4J [here](https://deeplearning4j.org/transfer-learning).

### Workspaces

Initialization methods often have an additional parameter named `workspaceMode`. For the majority of users you will not need to use this; however, if you have a large machine that has "beefy" specifications, you can pass `WorkspaceMode.SINGLE`. To learn more about workspaces, please see [this section](https://deeplearning4j.org/workspaces).

## Other Model Zoos

* [Caffe Model Zoo](https://github.com/BVLC/caffe/wiki/Model-Zoo) (The original model zoo ;)
* [Tensorflow Model Zoo](https://github.com/tensorflow/models)
* [PyTorch Vision Model Zoo](https://github.com/pytorch/vision/tree/master/torchvision)
* [Keras Model Zoo](https://github.com/fchollet/deep-learning-models)
* [MxNet Model Zoo](https://mxnet.apache.org/model_zoo/index.html)
* [CNTK Model Zoo](https://www.microsoft.com/en-us/cognitive-toolkit/features/model-gallery/)