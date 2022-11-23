# Ai

Discussed here are the features of the NovelAi drawn images. These differences are the ones that are currently being optimised and need to be optimised.

This section assumes that the illustration is either drawn by Ai or by a human and that the illustration contains a depiction of a face. The following tips are for reference only, as there is no clear division between human and Ai drawing, and the flaws in Ai illustrations are discussed more here.

The Img2Img technique is not discussed here; under Img2Img, the difference between human and Ai is no longer visible.

**The content of this section cannot be used as a basis for you to accuse others in forum**

Content translated from[human-or-ai-walkthrough](https://blog.oimo.io/2022/10/21/human-or-ai-walkthrough/)

## Cannot be based on

- Degree of completion

Illustrations by NovelAi are currently approaching or even surpassing the finish of real-life artists, so a cursory look is impossible to distinguish whether they are drawn by AI or not.

- Theme/style/atmosphere

By learning prompt editing, anyone can easily get away from NovelAi's default drawing style and get Ai to generate images that match the theme/drawing style (the more well-known the easier, e.g. Kyoto animation style). So it is no longer feasible to judge the drawing style characteristics based on the subject of the illustration.

## How to judge

Ai are not very good at keeping details consistent, whereas humans are very good at keeping details consistent.

### Consistency in the shape and highlights of the left and right eye

This is the most obvious feature. With humans, the shape and amount of left and right eye and highlights are consistent unless there is a specific reason, such as the character being in a special situation or special effects being used.

However, in the case of AI painting, the left and right eyes are almost certainly painted and highlighted differently. Whereas humans can understand that "the left and right eyes are physically the same shape, and in the same situation there is certainly consistency there", the AI cannot understand the principles behind the painting.

The most obvious of these is that "the highlight blends with the white of the eye and the pupil breaks up". Humans know that "first the eyeball, then the pupil, on which the surrounding light reflects to form the gloss", so it is taken for granted that "the shape of the pupil itself is not broken even if part of the pupil is blocked by the highlight", but the AI does not recognise any "logical relationship between the whites of the eyes, the pupil and the highlight", it only learns the end result. The unnatural deformation of the eye is another basis for judgement. Humans know that the eye is essentially a regular circle, but the AI does not.

Another feature of Ai's illustrations is that it usually changes the colour of the left and right eyes slightly. There are of course characters with different coloured left and right eyes, but in most cases these can be clearly identified as having different colours. It is one of the factors that can be used to determine whether a character appears to be the same colour at first glance, but is different on closer inspection.

However, it is not an important basis for this, as such illustrations are common. Also, it is natural for the colours of the left and right eyes to look different depending on their surroundings, so make no mistake about it.

### Consistency of the eyelashes of the left and right eyes

In illustrations drawn by humans, the eyelashes are usually symmetrical along the centre line of the face. However, in AI-generated illustrations, the number and position of eyelashes appearing on the left and right eyes are usually different.

However, even in human-generated illustrations, the shape of the eyelashes on the left and right eyes can differ to some extent, so it does not matter if the shape of the eyelashes on the left and right eyes are different. On the other hand, if the shape of the lashes is consistent from left to right, along with the eyes and highlights, then it is almost certainly an illustration drawn by a human.

### Symmetry in clothing

Humans know that "most clothes are made symmetrically"; the AI does not know this, but it knows to some extent that clothing designs tend to have similar shapes on the left and the right. 

As a result this happens.

```
The garment has a symmetrical design, but on closer inspection the left and right sides are clearly different at the topological level.
```

It is important to note that the features should be *at the topological (=connection of shapes, number of pieces, ......) be different *at the level of topology*, not *symmetrical or asymmetrical*. Even humans cannot draw symmetrical objects perfectly symmetrically and some distortion of size and shape will occur. Asymmetrical clothing designs are also common, so make no mistake about it.

This asymmetry can be applied not only to clothing but also to decorative items such as crowns and hair accessories.

### Repetition of shape consistency

This is also a key feature; Ai is not good at drawing the same shape in succession, even if it is a row of buttons on a garment.

Of course, even when drawn by a human, it can be slightly off, and sometimes deliberately not drawn neatly, so it's hard to tell. But at the very least, it is unlikely that an AI would deliberately arrange the same shapes at equal intervals as a human would.

### Simple geometric shapes

Artificial intelligence is not very good at drawing straight lines, regular circles and clean ellipses of a certain size. This is because the AI does not know the conception of these shapes. However, Ai is very good at drawing straight lines in the background, and the edges of buildings, pipes, etc. are often drawn as straight lines.

If simple, clear shapes appear in the illustration, they can be used to determine that the illustration was produced by a human.

### Functional consistency of artefacts

The parts of an artefact should inherently have "the meaning it was put there for" and "the function it provides".

Thus, **if there is an artefact** which is an aggregation of parts which are consistent in their meaning to provide a particular function, we can judge that the illustration was drawn by a human being.

It is important to note that **such inconsistencies should not be used to determine that an illustration was drawn by an AI** This is because humans can also draw such illustrations intentionally. This only means that an AI cannot draw mechanical illustrations that are good enough.

### Characters

Artificial intelligence cannot draw real letters correctly. **Most of the time, the letters it outputs** look somewhat futuristic, or as if they were invented in a completely different civilisation.

This is a very obvious feature, but it is important to note that **this should not be considered a feature of AI drawing**. It is naturally possible for humans to devise letters they have never seen before, and there are many illustrations in which mysterious letters have been devised in this way.

On the other hand, **if the letters are drawn in the 'right place' and in the 'proper order'**, this is a strong factor in determining that the illustration was drawn by a human. This is because even outputting well-formed text is difficult, and getting it in the right place is an extremely difficult task for today's artificial intelligence.

### Consistency in the role of lines

In human drawing, a line that draws the boundary of an object cannot suddenly become the boundary of another object in the middle of the line. Nor can lines shift between the inside and outside of an object.

Artificial intelligence does not know that lines should be consistent. When it encounters a similar pixel point, it easily takes the **Oh, I even! ** the idea of connecting lines logically and rendering them as a single line.

### Dissolve, blend and noise

This is a very common point in AI-generated illustrations. Internal/external judgements become blurred, causing objects to dissolve, blend with other objects, or produce unnatural noise.

Noise and dissolve can currently be fixed by oversampling and incremental training.

### Background colour

An otherwise empty space enclosed with hair or other objects (e.g. arms) has the potential for the AI to paint a background that is different from its surroundings. Often, the enclosed background is painted a slightly brighter colour than the rest of the space.

### Discontinuity

Humans understand that when an object is obscured by another object, **the same object persists beneath it**. Artificial intelligence does not understand that even a brief interruption of a hair may result in a loss of continuity between the objects before and after it.

### Details

Humans can understand that the size of an object is independent of its importance to the picture, so that even small objects, such as earrings or belts, are carefully drawn if they are an important element of the picture. The careful drawing of such small objects is a major factor in determining whether an illustration is drawn by a human being.
