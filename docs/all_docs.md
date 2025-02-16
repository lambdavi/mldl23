---
description: |
    API documentation for modules: main, fda_server, server, client, loss, style_transfer.

lang: en

classoption: oneside
geometry: margin=1in
papersize: a4

linkcolor: blue
links-as-notes: true
...


    
# Module `main` {#id}






    
## Functions


    
### Function `gen_clients` {#id}




>     def gen_clients(
>         args,
>         train_datasets,
>         test_datasets,
>         validation_datasets,
>         model
>     )


Divide the datasets in clients.

    
### Function `get_dataset_num_classes` {#id}




>     def get_dataset_num_classes(
>         dataset
>     )




    
### Function `get_datasets` {#id}




>     def get_datasets(
>         args
>     )


Function to get the datasets based on the args. It return three lists: train datasets, test_datasets and if necessary
validation datasets.

    
### Function `get_source_client` {#id}




>     def get_source_client(
>         args,
>         model
>     )


Function to get the clients based on the dataset. This function is only used in the fda setting. Returns None otehrwise. 


Args
-----=
<code>args</code>, <code>model</code> (pytorch)

Returns
-----=
list of one clients containing the source training set. This function is needed since the 'gen_clients' functions focuses
on the dataset split.

    
### Function `get_transforms` {#id}




>     def get_transforms(
>         args
>     )


Get the transformations based both on the dataset and the model.

    
### Function `main` {#id}




>     def main()




    
### Function `model_init` {#id}




>     def model_init(
>         args
>     )


Get the model based on the value of args.

    
### Function `set_metrics` {#id}




>     def set_metrics(
>         args
>     )


Get the metrics used to evaluate performance based on the task (determined by the model).

    
### Function `set_seed` {#id}




>     def set_seed(
>         random_seed
>     )







    
# Module `fda_server` {#id}







    
## Classes


    
### Class `FdaServer` {#id}




>     class FdaServer(
>         args,
>         source_dataset,
>         train_clients,
>         test_clients,
>         model,
>         metrics,
>         valid=False,
>         valid_clients=None
>     )


A class representing the Server where FDA (Fourier Domain Adaptation) is used.

The FDA server is responsible for coordinating the training process of the federated learning system.


Args
-----=
<code>args</code>: An object containing the arguments and configuration for the FDA server.

<code>source\_dataset</code>: The source dataset used for domain adaptation.

<code>train\_clients</code>: A list of training clients.

<code>test\_clients</code>: A list of test clients.

<code>model</code>: The initial source model.

<code>metrics</code>: A dictionary containing evaluation metrics.

<code>valid</code> (bool): Whether to include validation during training. Default is False.

<code>valid\_clients</code>: A list of validation clients. Required if <code>valid</code> is True. Default is None.







    
#### Methods


    
##### Method `aggregate` {#id}




>     def aggregate(
>         self,
>         updates
>     )


Aggregates the model updates received from the clients using FedAvg.


Args
-----=
<code>updates</code>: Model updates received from the clients. List of tuples.

Returns
-----=
<code>OrderedDict</code>
:   Aggregated model parameters.



    
##### Method `eval_train` {#id}




>     def eval_train(
>         self
>     )


This method handles the evaluation on the train clients

    
##### Method `eval_validation` {#id}




>     def eval_validation(
>         self
>     )


This method handles the evaluation on the validation client(s)

    
##### Method `extract_styles` {#id}




>     def extract_styles(
>         self
>     )


Extracts styles from the training clients and adds them to the style augmenter.

    
##### Method `predict` {#id}




>     def predict(
>         self,
>         image_path
>     )


Handles the the prediction. Outputs an image in the root directory.

Args: 
    <code>image\_path</code>: path to the image to predict.

    
##### Method `select_clients` {#id}




>     def select_clients(
>         self,
>         seed=None
>     )


Selects a subset of clients for a training round.


Args
-----=
<code>seed</code> (int): Random seed for client selection. Default is None.

Returns
-----=
<code>numpy.ndarray</code>
:   An array of selected clients for the training round.



    
##### Method `test` {#id}




>     def test(
>         self
>     )


This method handles the test on the test clients

    
##### Method `train` {#id}




>     def train(
>         self
>     )


This method orchestrates the training the evals and tests at rounds level.

    
##### Method `train_round` {#id}




>     def train_round(
>         self,
>         clients
>     )


Trains the model with the datasets of multiple clients in a federated training round.


Args
-----=
<code>clients</code>: A list of clients to train.

Returns
-----=
<code>list\[tuple]</code>
:   [(len_dataset, state_dictionary)]. Model updates gathered from the client to be aggregated.



    
##### Method `train_round_source` {#id}




>     def train_round_source(
>         self,
>         client
>     )


Trains a single round of the source model.


Args
-----=
<code>[client](#client "client")</code>: A client object containing the source dataset.

Returns
-----=
<code>tuple</code>
:   (len_dataset, dict_params). Model updates gathered from the client to be aggregated.



    
##### Method `train_source` {#id}




>     def train_source(
>         self
>     )


Trains the source model on the source dataset.



    
# Module `server` {#id}







    
## Classes


    
### Class `Server` {#id}




>     class Server(
>         args,
>         train_clients,
>         test_clients,
>         model,
>         metrics,
>         valid=False,
>         valid_clients=None
>     )


A class representing the Server for both centralized mode and distributed mode without FDA.

The Server is responsible for coordinating the training process of the system. It handles
the client-server framework.


Args
-----=
**```args```**
:   An object containing the arguments and configuration for the FDA server.


**```train_clients```**
:   A list of training clients.


**```test_clients```**
:   A list of test clients.


**```model```**
:   The initial source model.


**```metrics```**
:   A dictionary containing evaluation metrics.


**```valid```** :&ensp;<code>bool</code>
:   Whether to include validation during training. Default is False.


**```valid_clients```**
:   A list of validation clients. Required if <code>valid</code> is True. Default is None.









    
#### Methods


    
##### Method `aggregate` {#id}




>     def aggregate(
>         self,
>         updates
>     )


Aggregates the model updates received from the clients using FedAvg.


Args
-----=
<code>updates</code>: Model updates received from the clients. List of tuples.

Returns
-----=
<code>OrderedDict</code>
:   Aggregated model parameters.



    
##### Method `eval_train` {#id}




>     def eval_train(
>         self
>     )


This method handles the evaluation on the train clients

    
##### Method `eval_validation` {#id}




>     def eval_validation(
>         self
>     )


This method handles the evaluation on the validation client(s)

    
##### Method `predict` {#id}




>     def predict(
>         self,
>         image_path
>     )


Handles the the prediction. Outputs an image in the root directory.

Args: 
    <code>image\_path</code>: path to the image to predict.

    
##### Method `select_clients` {#id}




>     def select_clients(
>         self,
>         seed=None
>     )


Selects a subset of clients for a training round.


Args
-----=
**```seed```** :&ensp;<code>int</code>
:   Random seed for client selection. Default is None.



Returns
-----=
<code>numpy.ndarray</code>
:   An array of selected clients for the training round.



    
##### Method `test` {#id}




>     def test(
>         self
>     )


This method handles the test on the test clients

    
##### Method `train` {#id}




>     def train(
>         self
>     )


This method orchestrates the training the evals and tests at rounds level

    
##### Method `train_round` {#id}




>     def train_round(
>         self,
>         clients
>     )


This method trains the model with the dataset of the clients. It handles the training at single round level

Args
-----=
<code>clients</code>: list of all the clients to train

Returns
-----=
<code>list\[tuple]</code>
:   [(len_dataset, state_dictionary)]. Model updates gathered from the client to be aggregated.





    
# Module `client` {#id}







    
## Classes


    
### Class `Client` {#id}




>     class Client(
>         args,
>         dataset,
>         model,
>         test_client=False,
>         val=False
>     )


Client class for training and testing models on a specific dataset.

Args
-----=
<code>args</code>: Arguments object containing the client-specific configurations.

<code>dataset</code>: Dataset object for training and testing.

<code>model</code>: Model object to be trained and tested.

<code>test\_client</code> (bool): Flag indicating if the client is a test client.

<code>val</code> (bool): Flag indicating if validation should be performed.






    
#### Static methods


    
##### `Method update_metric` {#id}




>     def update_metric(
>         metric,
>         outputs,
>         labels
>     )


Update the evaluation metric with the model outputs and labels.


Args
-----=
<code>metric</code>: Metric object to be updated.

<code>outputs</code>: Model outputs.

<code>labels</code>: True labels.


    
#### Methods


    
##### Method `get_optimizer_and_scheduler` {#id}




>     def get_optimizer_and_scheduler(
>         self
>     )


Get the optimizer and scheduler based on the run configuration.


Returns
-----=
Optimizer object and scheduler object.

    
##### Method `plot_loss_miou` {#id}




>     def plot_loss_miou(
>         self
>     )


Plot the training mIoU and validation mIoU over epochs.

This method generates a line chart showing the training mIoU and validation mIoU
over the epochs. It plots the mIoU values stored in the <code>mious</code> attribute of the
<code>[Client](#client.Client "client.Client")</code> object.

The chart is saved as an image file named "miou_vs_miou.png".


Returns
-----=
None

    
##### Method `run_epoch` {#id}




>     def run_epoch(
>         self,
>         cur_epoch,
>         optimizer
>     )


Run a single epoch of training (on source/centralized dataset).


Args
-----=
<code>cur\_epoch</code>: Current epoch index.

<code>optimizer</code>: Optimizer object.

Returns
-----=
None

    
##### Method `run_epoch_pseudo` {#id}




>     def run_epoch_pseudo(
>         self,
>         cur_epoch,
>         optimizer,
>         crit,
>         red
>     )


Run a pseudo-epoch for self-training.


Args
-----=
<code>cur\_epoch</code>: Current epoch index.

<code>optimizer</code>: Optimizer object.

<code>crit</code>: Criterion object.

<code>red</code>: Reduction object.

Returns
-----=
Early stopping condition (if set) or None

    
##### Method `set_set_style_tf_fn` {#id}




>     def set_set_style_tf_fn(
>         self,
>         styleaug
>     )


Set the style transfer function for the client's dataset.


Args
-----=
<code>styleaug</code>: Style augmentation object.

Returns
-----=
None

    
##### Method `set_teacher` {#id}




>     def set_teacher(
>         self,
>         teacher_model
>     )


Set the teacher model for self-training.


Args
-----=
<code>teacher\_model</code>: Teacher model object.

    
##### Method `test` {#id}




>     def test(
>         self,
>         metric,
>         eval=None,
>         eval_dataset=None
>     )


Test the model on the client's dataset.


Args
-----=
<code>metric</code>: Evaluation metric object.
<code>eval</code>: Flag indicating if evaluation is being performed. Default: None
<code>eval\_dataset</code>: Evaluation dataset. Default: None

Returns
-----=
Mean IoU score if evaluation flag is set.

    
##### Method `train` {#id}




>     def train(
>         self,
>         eval_metric=None,
>         eval_datasets=None
>     )


This method locally trains the model with the dataset of the client. It handles the training at epochs level
(by calling the run_epoch method for each local epoch of training)

Args
-----=
<code>eval\_metric</code>: Evaluation metric object. Default: None
<code>eval\_datasets</code>: List of evaluation datasets. Default: None

Returns
-----=
Length of the local dataset, model's state dictionary.



    
# Module `loss` {#id}






    
## Functions


    
### Function `weight_test_loss` {#id}




>     def weight_test_loss(
>         losses
>     )


Computes the weighted test loss based on the given dictionary of losses.


Args
-----=
<code>losses</code> (dict): A dictionary containing the losses for different samples.

Returns
-----=
<code>float</code>
:   The weighted average of the losses.



    
### Function `weight_train_loss` {#id}




>     def weight_train_loss(
>         losses
>     )


Computes weighted training losses based on the given dictionary of losses.


Args
-----=
<code>losses</code> (dict): A dictionary containing the losses for different samples.

Returns
-----=
<code>dict</code>: A dictionary containing the weighted average of losses.


    
## Classes


    
### Class `EntropyLoss` {#id}




>     class EntropyLoss(
>         lambda_entropy=0.005,
>         num_classes=13,
>         **kwargs
>     )


Entropy loss.


Args
-----=
**```lambda_entropy```** :&ensp;<code>float</code>
:   Weighting factor for the entropy loss.


**```num_classes```** :&ensp;<code>int</code>
:   Number of classes.


Inherits from:
    nn.Module

Initializes internal Module state, shared by both nn.Module and ScriptModule.


    
#### Ancestors (in MRO)

* [torch.nn.modules.module.Module](#torch.nn.modules.module.Module)






    
#### Methods


    
##### Method `entropy_loss` {#id}




>     def entropy_loss(
>         self,
>         pred
>     )




    
##### Method `forward` {#id}




>     def forward(
>         self,
>         pred
>     ) ‑> Callable[..., Any]


Forward pass of the entropy loss.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted logits.



Returns
-----=
<code>torch.Tensor</code>
:   The calculated loss value.



    
### Class `IW_MaxSquareloss` {#id}




>     class IW_MaxSquareloss(
>         ignore_index=255,
>         ratio=0.2,
>         **kwargs
>     )


Implements the IW_MaxSquareloss loss function for image segmentation.


Args
-----=
<code>ignore\_index</code> (int): The index value to be ignored during loss calculation.
<code>ratio</code> (float): The ratio parameter for weight calculation.

Returns
-----=
<code>torch.Tensor</code>
:   The calculated loss value.


Initializes internal Module state, shared by both nn.Module and ScriptModule.


    
#### Ancestors (in MRO)

* [torch.nn.modules.module.Module](#torch.nn.modules.module.Module)



    
#### Class variables


    
##### Variable `requires_reduction` {#id}









    
#### Methods


    
##### Method `forward` {#id}




>     def forward(
>         self,
>         pred,
>         **kwargs
>     ) ‑> Callable[..., Any]


Defines the computation performed at every call.

Should be overridden by all subclasses.

**Note:** 
Although the recipe for forward pass needs to be defined within
this function, one should call the :class:<code>Module</code> instance afterwards
instead of this since the former takes care of running the
registered hooks while the latter silently ignores them.


    
### Class `SelfTrainingLoss` {#id}




>     class SelfTrainingLoss(
>         conf_th=0.9,
>         fraction=0.66,
>         ignore_index=255,
>         lambda_selftrain=1,
>         **kwargs
>     )


Implements the self-training loss for image segmentation.


Args
-----=
<code>conf\_th</code> (float): Confidence threshold for pseudo-labeling.
<code>fraction</code> (float): Fraction of top-k pixels to be considered for pseudo-labeling.
<code>ignore\_index</code> (int): The index value to be ignored during loss calculation.
<code>lambda\_selftrain</code> (float): Weighting factor for the self-training loss.

Returns
-----=
<code>torch.Tensor</code>
:   The calculated loss value.


Initializes internal Module state, shared by both nn.Module and ScriptModule.


    
#### Ancestors (in MRO)

* [torch.nn.modules.module.Module](#torch.nn.modules.module.Module)


    
#### Descendants

* [loss.SelfTrainingLossEntropy](#loss.SelfTrainingLossEntropy)


    
#### Class variables


    
##### Variable `requires_reduction` {#id}









    
#### Methods


    
##### Method `forward` {#id}




>     def forward(
>         self,
>         pred,
>         imgs=None,
>         seg=False
>     ) ‑> Callable[..., Any]


Forward pass of the self-training loss.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted logits.


**```imgs```** :&ensp;<code>torch.Tensor</code>
:   The input images (optional).


**```seg```** :&ensp;<code>bool</code>
:   Whether the prediction is segmentation logits.



Returns
-----=
<code>torch.Tensor</code>
:   The calculated loss value.



    
##### Method `get_batch_mask` {#id}




>     def get_batch_mask(
>         self,
>         pred,
>         pseudo_lab
>     )


Generates the mask for pseudo-labeling for a batch of samples.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted probabilities.


**```pseudo_lab```** :&ensp;<code>torch.Tensor</code>
:   The pseudo labels.



Returns
-----=
<code>torch.Tensor</code>
:   The generated batch mask.



    
##### Method `get_image_mask` {#id}




>     def get_image_mask(
>         self,
>         prob,
>         pseudo_lab
>     )


Generates the mask for pseudo-labeling based on confidence threshold and top-k fraction.


Args
-----=
**```prob```** :&ensp;<code>torch.Tensor</code>
:   The predicted probabilities.


**```pseudo_lab```** :&ensp;<code>torch.Tensor</code>
:   The pseudo labels.



Returns
-----=
<code>torch.Tensor</code>
:   The generated mask.



    
##### Method `get_pseudo_lab` {#id}




>     def get_pseudo_lab(
>         self,
>         pred,
>         imgs=None,
>         return_mask_fract=False,
>         model=None,
>         seg=False
>     )


Generates pseudo-labels for the given predictions and images.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted probabilities.


**```imgs```** :&ensp;<code>torch.Tensor</code>
:   The input images.


**```return_mask_fract```** :&ensp;<code>bool</code>
:   Whether to return the mask fraction.


**```model```**
:   The teacher model (optional).


**```seg```** :&ensp;<code>bool</code>
:   Whether the prediction is segmentation logits.



Returns
-----=
<code>torch.Tensor</code>
:   The generated pseudo-labels.



    
##### Method `set_teacher` {#id}




>     def set_teacher(
>         self,
>         model
>     )


Sets the teacher model for self-training.


Args
-----=
**```model```**
:   The teacher model.



Returns
-----=
None

    
### Class `SelfTrainingLossEntropy` {#id}




>     class SelfTrainingLossEntropy(
>         lambda_entropy=0.005,
>         **kwargs
>     )


Self-training loss with entropy regularization.


Args
-----=
**```lambda_entropy```** :&ensp;<code>float</code>
:   Weighting factor for the entropy regularization.


Inherits from:
    SelfTrainingLoss

Initializes internal Module state, shared by both nn.Module and ScriptModule.


    
#### Ancestors (in MRO)

* [loss.SelfTrainingLoss](#loss.SelfTrainingLoss)
* [torch.nn.modules.module.Module](#torch.nn.modules.module.Module)





    
#### Static methods


    
##### `Method entropy_loss` {#id}




>     def entropy_loss(
>         pred
>     )


Calculates the entropy loss.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted logits.



Returns
-----=
<code>torch.Tensor</code>
:   The calculated entropy loss.




    
#### Methods


    
##### Method `cross_entropy` {#id}




>     def cross_entropy(
>         self,
>         pred,
>         imgs=None
>     )


Calculates the cross-entropy loss.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted logits.


**```imgs```** :&ensp;<code>torch.Tensor</code>
:   The input images.



Returns
-----=
<code>torch.Tensor</code>
:   The calculated cross-entropy loss.



    
##### Method `forward` {#id}




>     def forward(
>         self,
>         pred,
>         imgs=None
>     ) ‑> Callable[..., Any]


Forward pass of the self-training loss with entropy regularization.


Args
-----=
**```pred```** :&ensp;<code>torch.Tensor</code>
:   The predicted logits.


**```imgs```** :&ensp;<code>torch.Tensor</code>
:   The input images.



Returns
-----=
<code>torch.Tensor</code>
:   The calculated loss value.





    
# Module `style_transfer` {#id}







    
## Classes


    
### Class `StyleAugment` {#id}




>     class StyleAugment(
>         n_images_per_style=10,
>         L=0.1,
>         size=(1024, 512),
>         b=None
>     )


Class for applying FDA (Fouried Domain Adaptation) style augmentation to images.


Args
-----=
**```n_images_per_style```** :&ensp;<code>int</code>
:   Number of images per style.


**```L```** :&ensp;<code>float</code>
:   Parameter for computing the size of the style patch.


**```size```** :&ensp;<code>tuple</code>
:   Desired size of the images after preprocessing.


**```b```** :&ensp;<code>int</code>
:   Size of the style patch.



Methods
-----=
preprocess(self, x): Preprocesses the input image.
deprocess(self, x, size): Deprocesses the input image.
add_style(self, loader, multiple_styles=False, name=None): Adds styles to the style pool.
_extract_style(self, img_np): Extracts the style from an image.
compute_size(self, amp_shift): Computes the size of the style patch.
apply_style(self, image): Applies a random style to the input image.
_apply_style(self, img): Applies a specific style to the input image.
test(self, images_np, images_target_np=None, size=None): Tests the style augmentation on input images.







    
#### Methods


    
##### Method `add_style` {#id}




>     def add_style(
>         self,
>         loader,
>         multiple_styles=False,
>         name=None
>     )


Adds styles to the style pool.


Args
-----=
**```loader```**
:   Data loader for loading images.


**```multiple_styles```** :&ensp;<code>bool</code>
:   Whether to add multiple styles per image.


**```name```** :&ensp;<code>str</code>
:   Name of the style.



Returns
-----=
None

    
##### Method `apply_style` {#id}




>     def apply_style(
>         self,
>         image
>     )




    
##### Method `compute_size` {#id}




>     def compute_size(
>         self,
>         amp_shift
>     )


Computes the size of the style patch.


Args
-----=
**```amp_shift```**
:   Shifted amplitude spectrum.



Returns
-----=
<code>tuple</code>
:   Size parameters (h1, h2, w1, w2) of the style patch.



    
##### Method `deprocess` {#id}




>     def deprocess(
>         self,
>         x,
>         size
>     )


Deprocesses the input image.


Args
-----=
**```x```**
:   Input image.


**```size```**
:   Desired size of the output image.



Returns
-----=
<code>PIL.Image.Image</code>
:   Deprocessed image.



    
##### Method `preprocess` {#id}




>     def preprocess(
>         self,
>         x
>     )


Preprocesses the input image.


Args
-----=
**```x```**
:   Input image.



Returns
-----=
<code>np.ndarray</code>
:   Preprocessed image.



    
##### Method `test` {#id}




>     def test(
>         self,
>         images_np,
>         images_target_np=None,
>         size=None
>     )


Tests the style augmentation on input images.


Args
-----=
**```images_np```**
:   Input images.


**```images_target_np```**
:   Target images (optional).


**```size```**
:   Desired size of the images (optional).



Returns
-----=
None


-----
Generated by *pdoc* 0.10.0 (<https://pdoc3.github.io>).
