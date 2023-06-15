# Edge2Starfish 
Coding3 Final Project

[画画然后生成图片的gif]

## Design Concept
I was mostly inspired by edge2cat (I really love cats) and thought it was really interesting to just draw an outline and create an image. So I chose to complete a project based on Pix2Pix. On the other hand, I chose starfish as the dataset because of its clear edges and rich colors. I think that can be a good place to start.
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/3a5448ac-36c2-4670-a55c-d3d0683e50a7)


## Development Processes

### Source Code 
<b>TensorFlow Pix2Pix Colab Tutorial:</b> https://www.tensorflow.org/tutorials/generative/pix2pix 

Based on this, I changed the original data set on its page to edges2shoes and tried to run it. Fortunately, the entire code can run completely, and there is a result where you can see some initial effects. <a href="https://colab.research.google.com/drive/1eLjt6m9METNeEWnxjD2yZJuRZhdEb-0S#scrollTo=wozqyTh2wmCu"> Colab Link in this part </a>
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/1ab40a4d-2ec3-4aa6-8728-997dd29ba63c)


### Create Starfish Dataset
<b>Dataset Source:</b> https://images.cv/download/starfish/2042/CALL_FROM_SEARCH/%22starfish%22
   
After looking at the dataset example in edges2shoes, I decided to make a starfish dataset modeled after its dataset. And realized that I needed at least two images, one of a close-up of a starfish and one of an outline of a starfish. 
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/d6d5e6e9-8d60-4ba7-baca-ff6159f4dfea)

But the dataset I downloaded only has images, which means I need to matte and get the edges of the image. To do this I used two online tools：<a href="https://www.remove.bg/">Removebg</a> and <a href="https://www.photo-kako.com/en/edge/">Photokako</a>.  

The process of making the whole dataset is as follows: I created a 256*256 artboard with a white background in Photoshop, dragged the images from the downloaded dataset into removebg to remove the background and keep only the starfish, then put them into Photoshop, adjusted the size and position to make them as consistent and centered as possible.
![fd7e260d3bf3ff7fa64c3303b432321](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/4943a685-e3ea-482d-ba98-49539e3101f4)


On the other hand, I put the original picture into Photokako and adjusted the parameters according to the different conditions of each picture to achieve my ideal effect: there are obvious external contours, while the inner pattern is not very messy.
![d42ab7372e9e33b1d0dbf4a41008fc2](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/caf57397-e1bf-4490-9a40-f971778444af)

And save them separately in a folder. I ended up with 160 pairs of images, 150 as a training set, and 10 as a validation set.
![f846ba220542820662843ca8e424618](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/214ef410-556d-4325-9a37-3f5b741ca170)




### Change the Code to Fit the Dataset
Since I made the dataset myself and now run it on Colab, I needed to read the Google Drive files in Colab. So I borrowed <a href="https://stackoverflow.com/questions/48376580/how-to-read-data-in-google-colab-from-my-google-drive">this guy's solution</a> for that.

However, I also found a new problem. Since I chose grayscale mode for the convenience of the image when obtaining the edge contour at the beginning, there is a problem that the format of the original model does not match the type required by the original model when importing the image. For this reason, I inquired the introduction of <a href="https://www.tensorflow.org/api_docs/python/tf/io/decode_jpeg#args">tf.io.decode_jpeg</a> and solved this problem by adding "channels=3".

And because I didn't put the two graphs together like the data set it gave me, I needed to change the load() function in its original code. (The reason I didn't merge was because I was afraid that some of my drawings were not good enough to be replaced later, but it turned out to be a wrong decision) In order to achieve the same effect as the original code (enter one image address to get two images), I used GPT to write a generate_filenames() for me, which does: input "1.jpg" and return "1.jpg" and "1e.jpg" (the names I gave the pair of images).
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/3f95c368-5a47-413a-a1f1-a4ae11a471dd)

However, this method produced many errors in the future and was eventually abandoned by me. Because I'm trying to use python string manipulation during the execution of a TensorFlow diagram, and this reports <i>AttributeError: 'Tensor' object has no attribute 'rsplit'</i>. <a href="https://colab.research.google.com/drive/1fGgDccyaVjzfdjEKsx9XFEMegVQgizEr#scrollTo=Z9ucMj2dL5aS">The Whole Process of this Part</a>

### Optimized Dataset
After repeated revisions, I decided to give up this plan and chose to merge the two graphs into the same shape as the original data set.I achieved this goal with the help of GPT (based on the code it provided and made some changes on it).
![e86afbd30bb01d4d905d3933059cb43](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/8ea242ce-3a21-4840-a6fd-2af340c3ba10)












