# Assignment 2 reprot

## Genetic algoritm results:
![](https://i.imgur.com/HdHQQwf.gif)
![](https://i.imgur.com/dYMXywi.gif)
![](https://i.imgur.com/pAPGq6J.gif)
![](https://i.imgur.com/G7GXwdE.gif)
![](https://i.imgur.com/VXxTCLy.gif)
![](https://i.imgur.com/NbXlOmD.gif)
![](https://i.imgur.com/82EDLNV.gif)


## Results
## What is the representation in your algorithm?
Vector with Image objects* <br>
*objects are stored as images to reduce time taken by mutation and selection phases.

### Chromosomes
1. Chromosomes are represented as image objects with geometric figures drawn
2. New chromosomes are represented as empty images 
```python
target_image = Image.open('test.png')
target_image = np.array(target_image, np.int16)
empty_image = Image.new("RGB", (512,512), (255,255,255))
fit = fitness_function(target_image, empty_image)
new_population = [Gene(empty_image, target_image, fitness_value)]
```

## Which selection mechanism is being used?
### fitness based selection mechanism
Replace with a member of a sample of mutations if better than parent
For every chromosome fitness value is calculated and only n-best survives
```python
def get_the_best(population, n=1):
    parents = sorted(population, key=lambda x: fitness_function(target, x))[:n]
    return parents
```
#### Example 
For image: <img src="https://i.imgur.com/19PdNuf.png" width=100>

The first from this results will be chosen as it covers more image and as a result reduces error and has better fitness value.

<img src="https://i.imgur.com/GoChmYz.png" width=100></img><img src="https://i.imgur.com/TqkrVGF.png" width=100></img><img src="https://i.imgur.com/ZcOTlfk.png" width=90></img>

## Which image manipulation techniques have you applied?

### Plotting ellipsis
<img src="https://i.imgur.com/vQEcUdO.png" width=200></img>
### Plotting triangles
<img src="https://i.imgur.com/rDfAoHk.png" width=200></img>

## What is the fitness function?
sum of module of differences for each collor at each pixel which relfects how different images are
```python
def fitness_function(target, generated):
    target_pixels = target
    generated_pixels = np.array(generated, np.int16)
    return np.sum(np.abs(generated_pixels - target_pixels))
```
#### Example 
For image: <img src="https://i.imgur.com/19PdNuf.png" width=100>

Generated images:

<img src="https://i.imgur.com/Jd6oKER.png" width=100></img><img src="https://i.imgur.com/7eu76cr.png" width=100></img>

Fitness values:

<p style="word-spacing: 40px;">3993054 740396</p>

## Explain the Crossover function.
I used simple crossover as it is 

![](https://i.imgur.com/JAOTRpL.png)

1. First half of chromosome is filled with circles 
2. Second half of chromosome is filled with triangles
3. We merge 2 chromosomes by merging different halfs and choosing with the best score


### Example with crossover:
##### Image where crossover shows better results

#### Without crossover
![](https://i.imgur.com/NndJDvu.gif)
#### With crossover
![](https://i.imgur.com/DY5IiVO.gif)

#### Compare 2 images
<img src="https://i.imgur.com/TFkTZzb.png" width=350></img><img src="https://i.imgur.com/XsGylhs.png" width=350></img>

On crossover image mix of circles and triangles made possible to express more detailed forms

I did not use crossover for final results, because, in my opinion, better results were achived using only ellipses


```python
def crossover(new_circles, new_triangles):
    new = []
    best_triangle = get_the_best(new_triangles)
    best_circle = get_the_best(new_circles)
    best_circle.put_triangle(best_triangle.last_triangle)
    return best_circle
```





## Explain Mutation criteria.
Every parent produces 55 new chromosomes

### Mutation is done for every parent as following:
1. Random points are chosen
2. Color is chosen from original image
3. ellipse/triangle is drawn
4. fitness value is recalculated
5. results are saved as new chromosomes
#### Example 
Using ellipses
<img src="https://i.imgur.com/K8QHYxG.gif" width=300></img>
Using triangles
<img src="https://i.imgur.com/gPfVi0i.gif" width=300></img>

For me, ellipses produced better results, so I used them to produce images

mutation is done by put_figure of Gene class.

Fitness function is updated only for changed rectangle to increases performance.
```python
# figure 1 - ellipse, 2 - triangle
def put_figure(self, figure):
    canvas = ImageDraw.Draw(self.img, "RGB")
    if figure == 1:
        place = get_random_item(2)
        min_i, min_j = place[0]
        max_i, max_j = place[1]
        y, x, r = min_j, min_i, max([max_j - min_j, max_i - min_i])
        previous_score = self.get_diff_at_rectangle(y, x, r)
        i, j = get_pixel_coord_from_center(place)
        color = tuple(self.target[j, i])
        canvas.ellipse(place, fill=color)
    else:
        place = get_random_item(3)
        min_i, min_j = min([xy[0] for xy in place]), min([xy[1] for xy in place])
        max_i, max_j = max([xy[0] for xy in place]), max([xy[1] for xy in place])
        y, x, r = min_j, min_i, max([max_j - min_j, max_i - min_i])
        previous_score = self.get_diff_at_rectangle(y, x, r)
        i, j = get_pixel_coord_from_center(place)
        color = tuple(self.target[j, i])
        canvas.polygon(place, fill=color)
    new_score = self.get_diff_at_rectangle(y, x, r)
    self.fitness_value -= previous_score - new_score
    return self
```
Mutation is done is done for every parent
```python
def group_mutation(best_gene, num, figure=2):
    new = [best_gene.copy().put_figure(figure) for i in range(num)]
    return new
```


## What is art for you/How would you define art/what is your perception of art?
**Art** - any manifestation of personality that is expressed in written or oral form.

**My perception of art** - painting that are appreciated for their beauty.

## Describe the artistic aspect of your output images/why do you consider the output image as a piece of art?
1. Images have smooth shapes and soft transitions between colors that looks pretty
2. Building blocks of generated images are ellipses and triangles that looks like brush strokes that artists use

## Provide examples of images.
![](https://i.imgur.com/6bYJoA1.png)
![](https://i.imgur.com/gu311xS.png)
![](https://i.imgur.com/8pIE3R0.png)
![](https://i.imgur.com/xyPSPMc.png)
![](https://i.imgur.com/VUnmEVN.png)
![](https://i.imgur.com/9EPtzlm.png)
![](https://i.imgur.com/a01kZ4t.png)


