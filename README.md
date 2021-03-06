### Developmental approach to learning joint attention in a Nao robot
Learn to establish joint attention in a Nao robot, by following the gaze of a person to locate an intersting object, in this case a ball.
It uses [gaze following implemented in (mat)caffe]](http://gazefollow.csail.mit.edu/), face recognition and ball detection in OpenCV, and reinforcement learning (Q-learning) with OpenAI's Gym.

### Installation
- Install the dependencies listed below
- Clone or download this project
- Setup as listed below

#### Dependencies
The code is writen for pyhton 2.7. Futher dependencies are:
- gym 0.10.5
- numpy 1.14.5
- opencv-python 3.4.1.15
- pandas 0.23.1
- NaoQi
- matlabengineforpython
- matcaffe
Note that the given version were used during development however, other version might also work.

#### Setup of code
In order to run the gaze following with matcaffe, you need to add the path to your matcaffe folder in `follow_gaze.m` and `predict_gaze.m`:  
E.g. add: `addpath(genpath('/path/to/your/caffe/matlab'));`.

Also change the ip variable on line 21 of `Pipeline.py` to the ip of your nao robot.  
Then run `python Pipeline.py`.



### Run the code
You can run the code by running Pipeline.py. However, before running you need to set some path and the robot IP.
In Pipeline.py set the ip in line 21 to the ip of your nao robot.
In order to run the gaze following with matcaffe you need to add the path to your matcaffe the follow_gaze.m .
Add: addpath(genpath('/path/to/your/caffe/matlab'));
Also add the same path to the predict_gaze.m .
After both paths to matcaffe have been set in the follow_gaze.m and the predict_gaze.m file you can run the
code with Pipeline.py.  
  
By default it will use the trained Q-values. If this is not desired, remove the last line of `statistics.csv`.

###
The training process has been recorded, and can be seen found by clicking on the image below.  

[![Training video](https://img.youtube.com/vi/BH4QQcVaMp0/0.jpg)](https://www.youtube.com/watch?v=BH4QQcVaMp0)

### Architecture
![Architecture](https://github.com/KochPJ/follow_gaze_developmental_robotics/blob/master/architecture2.png  "Architecture")

In the image above a rough outline of the implemented architecture has been visualized.
We used object oriented programming for coding the developmental joint attention model.

Each block in the memory corresponds to an class, which can be found in a seperate file.
These classes are:
- Memory
- Decision
- Learning
- Motivation
- Perception
  - Camera
  - Ball
  - Face
  - Gaze
- Motorcontrol

The `Pipeline.py` file contains the main loop, and uses these classes to run an infinite number of trials, if not stopped by the user.

Instantiating these classes in the Pipeline, the camera class is setup first.
This camera class is passed to the memory module, such that every module with access to the memory can also request an image update.
The remaining classes are then each passed the memory object, which contains the settings for each module, the camera object for requesting new frames, and can be used to pass variables to other modules.
The pipeline calls the functions of the created objects, where every function is run by the pipeline.
Inside the pipeline it starts out by initializing and setup the robot proxies and objects. Afterwards, the main loop starts where the main pipeline is executed. The loop executes the following steps:
- setup the current trial
- while the trial is not done:
  - get a face in the image
  - get the balls in the image
  - get the motivation of the robot
  - make a decision based on the motivation and learned Q-values
  - execute the decision
- safe learned Q-values to a csv file (long term memory)
- waiting for terminal input instructions (continue new trial, or shutdown)

### References
The [Gaze Following model](http://gazefollow.csail.mit.edu/) used was created by A. Recasens*, A. Khosla*, C. Vondrick and A. Torralba. See:
Where are they looking?
A. Recasens*, A. Khosla*, C. Vondrick and A. Torralba
Advances in Neural Information Processing Systems (NIPS), 2015
