# Edge2Starfish 
Coding3 Final Project

## Design Concept
I was mostly inspired by edge2cat (I really love cats) and thought it was really interesting to just draw an outline and create an image. So I chose to complete a project based on pix2pix. On the other hand, I chose starfish as the dataset because of its clear edges and rich colors. I think that can be a good place to start.

## Development Processes

### Source Code 
<b>TensorFlow Pix2Pix Colab Tutorial:</b> https://www.tensorflow.org/tutorials/generative/pix2pix 

Based on this, I changed the original data set in its page to edges2shoes and tried to run it. Fortunately, the entire code can run completely, and there is a result where you can see some initial effects. <a href="https://colab.research.google.com/drive/1eLjt6m9METNeEWnxjD2yZJuRZhdEb-0S#scrollTo=wozqyTh2wmCu"> Colab Link in this part </a>

### Create Starfish Dataset
<b>Dataset Source:</b> https://images.cv/download/starfish/2042/CALL_FROM_SEARCH/%22starfish%22
   
After looking at the dataset example in edges2shoes, I decided to make a starfish dataset modeled after its dataset. And realized that I needed at least two images, one of a close-up of a starfish and one of an outline of a starfish. 

But the dataset I downloaded only has images, which means I need to matte and get the edges of the image. To do this I used two online tools：<a href="https://www.remove.bg/">Removebg</a> and <a href="https://www.photo-kako.com/en/edge/">Photokako</a>.  

The process of making the whole dataset is as follows: I created a 256*256 artboard with a white background in Photoshop, dragged the images from the downloaded dataset into removebg to remove the background and keep only the starfish, then put them into Photoshop, adjusted the size and position to make them as consistent and centered as possible.

On the other hand, I put the original picture into Photokako and adjust the parameters according to the different conditions of each picture to achieve my ideal effect: there are obvious external contours, while the inner pattern is not very messy.

And save them separately in a folder. I ended up with 160 pairs of images, 150 as a training set and 10 as a validation set.

### Change the Code to Fit the Dataset



