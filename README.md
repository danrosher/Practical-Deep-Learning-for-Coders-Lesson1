Practical-Deep-Learning-for-Coders-Lesson1
================

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

This file will become your README and also the index of your
documentation.

## Is it a bird

``` sh
pip install Practical_Deep_Learning_for_Coders_Lesson1
```

``` python
from fastbook import *
```

``` python
urls = search_images_ddg('bird photos', max_images=1)
len(urls),urls[0]
```

    (1, 'http://www.birdwallpapers.com/wallpapers/bird-photos.jpg')

``` python
dest=Path('bird.jpg')
download_url(urls[0],dest,show_progress=False)
```

    Path('bird.jpg')

``` python
im = Image.open(dest)
im.to_thumb(256,256)
```

![](index_files/figure-commonmark/cell-5-output-1.png)

``` python
searches = 'forest','bird'
path = Path('bird_or_not')

for o in searches:
    dest = (path/o)
    dest.mkdir(exist_ok=True, parents=True)
    results = search_images_ddg(f'{o} photo',max_images=20)
    download_images(dest, urls=results[:20])        
    resize_images(dest, max_size=400, dest=dest)
```

``` python
failed = verify_images(get_image_files(path))
failed.map(Path.unlink)
```

    (#0) []

``` python
dls = DataBlock(
    blocks=(ImageBlock, CategoryBlock), 
    get_items=get_image_files, 
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=[Resize(192, method='squish')]
).dataloaders(path, bs=32)

dls.show_batch(max_n=6)
```

![](index_files/figure-commonmark/cell-8-output-1.png)

``` python
learn = vision_learner(dls, resnet18, metrics=error_rate)
learn.fine_tune(3)
```

<style>
    /* Turns off some styling */
    progress {
        /* gets rid of default border in Firefox and Opera. */
        border: none;
        /* Needs to be in here for Safari polyfill so background images work as expected. */
        background-size: auto;
    }
    progress:not([value]), progress:not([value])::-webkit-progress-bar {
        background: repeating-linear-gradient(45deg, #7e7e7e, #7e7e7e 10px, #5c5c5c 10px, #5c5c5c 20px);
    }
    .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
        background: #F44336;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>error_rate</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1.577089</td>
      <td>2.165102</td>
      <td>0.571429</td>
      <td>00:01</td>
    </tr>
  </tbody>
</table>

<style>
    /* Turns off some styling */
    progress {
        /* gets rid of default border in Firefox and Opera. */
        border: none;
        /* Needs to be in here for Safari polyfill so background images work as expected. */
        background-size: auto;
    }
    progress:not([value]), progress:not([value])::-webkit-progress-bar {
        background: repeating-linear-gradient(45deg, #7e7e7e, #7e7e7e 10px, #5c5c5c 10px, #5c5c5c 20px);
    }
    .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
        background: #F44336;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>error_rate</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1.055121</td>
      <td>1.730857</td>
      <td>0.571429</td>
      <td>00:01</td>
    </tr>
    <tr>
      <td>1</td>
      <td>0.770361</td>
      <td>0.968586</td>
      <td>0.428571</td>
      <td>00:01</td>
    </tr>
    <tr>
      <td>2</td>
      <td>0.525786</td>
      <td>0.445994</td>
      <td>0.285714</td>
      <td>00:01</td>
    </tr>
  </tbody>
</table>

``` python
is_bird,_,probs = learn.predict(PILImage.create('bird.jpg'))
print(f"This is a: {is_bird}.")
print(f"Probability it's a bird: {probs[0]:.4f}")
```

<style>
    /* Turns off some styling */
    progress {
        /* gets rid of default border in Firefox and Opera. */
        border: none;
        /* Needs to be in here for Safari polyfill so background images work as expected. */
        background-size: auto;
    }
    progress:not([value]), progress:not([value])::-webkit-progress-bar {
        background: repeating-linear-gradient(45deg, #7e7e7e, #7e7e7e 10px, #5c5c5c 10px, #5c5c5c 20px);
    }
    .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
        background: #F44336;
    }
</style>

    This is a: bird.
    Probability it's a bird: 1.0000
