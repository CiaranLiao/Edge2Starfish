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

### Train the Model and Export
In this run, I didn't make any changes except to the part that fetched the data set, and I reduced BUFFER_SIZE to 4 because the number of my dataset was too small. Here's what happened after training 40,000 steps <a href="https://colab.research.google.com/drive/1sfp6xax3Y3VM4c7K4crBVvSsQGYgRapQ#scrollTo=ESagoGltwDtQ">and the process about this part</a>：

![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/31d62881-282b-49e2-a473-0326692ea713)
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/2fceaaef-9dc4-4192-8ed9-1c194fa2ba5e)
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/0dc456cb-3473-4791-9ac9-b9f9fcb0057c)
![image](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/b7366624-3b59-4472-8933-fbc38778247a)

I think this result is surprising enough, because I only used 150 pairs of images as the maximum data set, and edge2shoes has 50,000 images as the training set. In order to preserve this precious outcome, I went on to ask GPT how to export and download this model locally and how to use it on my computer. After a few more change and iterations, I was able to draw an outline and have the model produce a picture of a starfish for me:  

![33fb1cd4c65682f0a7bd4e662853304](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/5b0cc2cb-9eed-4099-bc47-6f7329483680)
![7afaae4e0b4ef19bf1e584bbbf69792](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/9de7c31f-d2f0-4243-9087-d33825ae63ea)
![ba48260ed1738cdf0e68a074c6035f9](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/fe461979-3697-44bc-b0dc-ae8615711f91)
![667b26f68fb32a81aedfc99a211e66e](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/8402d510-b42c-40d2-98b9-b6f24c3b189a)
![99c001e77d8485409c2f577adc63199](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/153b0283-e0c5-4755-9e11-ba0cb57bf84e)

### Further Attempt

While basically completing my initial idea, I want to continue to explore the effect of changing some of these parameters on the resulting image. To do this, I mainly tried tweaking the BUFFER_SIZE, optimizer, and activation in the generator. <a href="">the record about this part</a>

To be honest, this part was a big surprise to me, because the images generated were random and interesting every time. After removing the goal of "must become like a real starfish", every random form of it can bring me a new feeling, which throws away the sense of reality, full of confusion and strong conflict, and gives me a great inspiration.

![1ad4a57d90804942e389b763bb8fcb6](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/55588d4b-02a7-4a18-a605-cecb53eb7768)
![f732c0c150fdba26224f06da59b9e8a](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/bdd2ad66-e765-42e7-903f-9d95a961c6e2)
![304ed62d6d0d51b5f497e46b01fbe5c](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/1c656492-8134-41d6-bed0-0c5f04fd4131)
![44068e084863f2ab9d854277de3a1b9](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/40b72b38-e43a-499c-b24b-ad433c2f22b0)
![e47bcc5036bcc5f0d2753f236d17236](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/476da413-baec-4169-a8aa-0c980dc711f0)
![73805e3e3ffd50cdf6dcab7482529fb](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/dffce1de-4bf0-42ef-96f1-b51a5351161c)
![c078b46eb01fcef90ea2412558d1051](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/f223da61-7186-4470-9da0-58f27a81f3c7)
![37d9e9ab6941df04b3b01ffce6a90bc](https://github.com/CiaranLiao/Edge2Starfish/assets/53254700/3df3c8d3-e40c-4ccc-b1bf-6098fceb49e2)












