# Fenrir - Let your code be swallowed by the fierce wolf
<img src="static/images/fenrir.jpg" alt="IStable diffusion generated image of a black wolf with a glowing yellow orb in its yawsmage" width="300" />


# Boilerplate is booring

I get it, you hate to write boilerplate code. Just writing `from cool_config import YAMLConfigParser; from awesome_training import Trainer; from best_deep_models import Model; from saassy_opt import CloudExperimentTracker` you get to train your model really fast by just inherit `Model`, configure it with your `YAML` files, set up your credentials with `CloudExperimentTracker` and just make everything work magically with `Trainer`. Everything is awesome!

The problem comes two years later when some pesky researcher would love to use your code on their M3 Macbook, but they absolutely must use most recent version of `awesome_training`, which deprecated the support for `best_deep_model` a year ago and suddenly there's no way to resolve your dependency on the now depricated `Model` and your code is doomed to waste away in the forgotten corners of github.

*dependencies are putting on technical debt* - when you solve your problem by depending on an external library, your code is now coupled to it and in a fast moving field as machine learning, the thing you depend on might change really fast. Worst case, the package you depend on chooses to also take on dependencies and sooner or later you might end up with the situation that your dependency graph can't be resolved.

# But I don't want to write boilerplate!

It's very reasonable to not want to waste hours writing the same kind of code for each project. The alternative to having a "dynamic" dependency on a library is to "statically" include it which has been a tried solution in software engineering for a long time. Translating this to the setting of building machine learning experiments, instead of relying on an external package from PyPI or github, you *copy* the code into your project.

Functionally, you are in a similar situation as if you dependen on an external library (risk of bugs etc.) but your code will never stop working because the boilerplate-removing framework you had as a dependency messes up the dependency graph.

# Fenrir - opinionated ML quick start

A quick start framework form ML might want to solve a slew of things. This framework focuses on making it easy to make FAIR ML experiments. We try to follow standard structure and minimally solve the problem of running ML experiments (while trying to stay agnostic to the frameworks you might want to use: e.g. Pytorch or Tensorflow, optuna or hyperopt).

These are the specific problems we try to adress:

 - Configuration
 - Hyper parameter optimization
 - Experiment tracking

As a user, you make a copy of this code and fill in specific parts. 

## Structure
The code is structure in a python package called `fenrir`. It has a `setup.py` file which you can edit with the details of your project.

By default, fenrir tries to depend on as few non-standard packages as possible. Depending on what frameworks you would like to use, you will need to set up your own environments.

### Configuration

Configuration in fenrir is based on python files. You write your config by creating python objects inside files and the framework will load the objects in the files you give it.

### Model selection

Fenrir tries to give you some commonly used strategies for doing better model selection. By default the framework excepts you to do k-fold cross validation, but you might want to switch this out to other kind of subsampling strategies (e.g. group-based or stratified sampling). 

Fenrir also has a built in minimal hyper parameter optimization framework which will simply do random sampling of the hyper parameter space, but also contains an optional optuna-based backend which supports more sophisitcated search strategies (e.g. Parzen Tree esitmators). 

### Dataset definitions

Fenrir is agnostic to how your datasets interact with your models, its up to you to decide on how the model treats the dataset. Fenrir give you the pipeline which feeds a dataset to a model and finds suitable hyper parameters.


### Model definitions

Fenrir is also agnostic to how the model is implemented, it doesn't care wheter its a Pytorch model, a scikit learn regressor or just a statistics.mean. We provide foundational training loop for pytorch models, which you are expected to modify to do what you'd like.

