# Cattle-ROBB Dataset
## Motivation
These datasets use OBBs or polygons to annotate objects. 
The polygons are converted to minimum enclosing OBBs before being fed into detection models. 
The OBB is often represented as $[x_c, y_c, w, h, \theta]$ where $(x_c, y_c)$ is box center, $(w, h)$ is for width and height, 
and $\theta$ refers to how much the rectangle rotates in range $(-\pi/2,0]$. 
The OBB can also be represented as four corner points $[x_1, y_1, x_2, y_2, x_3, y_3, x_4, y_4]$. 
In fact, 
\textbf{the orientation $\theta$ is just a parameter for the oriented box definition instead of indicating the intrinsic orientation of the target object}. 

However, the livestock has intrinsic orientations in $[0, 2\pi)$, usually represented by head or body direction in stockbreeding context.  
This intrinsic orientation is a very fundamental indicator in developing intelligent stockbreeding downstream applications, such as body size estimation, and eating activity monitoring. 
Moreover, usually, only a single species is kept on one animal farm. 
Hence, 
we re-annotate a single-category top-view cattle object detection dataset with real-oriented bounding boxes (ROBB) annotation. 
The ROBB extends OBB by explicitly annotating the intrinsic orientations of cattle individuals to oriented bounding boxes. 
In other words, the orientation of ROBB represents the intrinsic orientation of cattle body within the range $[\bm{0}, \bm{2\pi})$ as shown in \cref{fig1e}. 
These orientations marked by red arrows are assigned with a physical meaning instead of being the parameter for box definition. 
This ROBB annotation on the non-rigid cattle bodies makes the detection task more elaborate and yet challenging. 
## Implementation
We extend the Cows2021 dataset\cite{IDEN_OPEN_GAO2021} by re-annotating the entire cattle body with ROBB. In the Cows2021 dataset, only the cattle torso area is annotated without including the head and neck. The torso ROI is detected by oriented detectors and used for cattle identification. But the entire cattle body including head and neck is also important to many downstream intelligent stockbreeding applications. For example, the entire body especially the head must be taken into consideration in eating and drinking behavior analysis. We can only define these behaviors empirically with the entire body information. Therefore we re-annotated images in the Cows2021 dataset by enclosing the entire cattle body with ROBB. We select 5587 images with at least one entire cattle body from the total of 10402 images in Cows2021. The original images are resolved at $1280 \times 720$, but for ROBB annotation the corner points of bounding boxes may locate outside the image if the cattle bodies are near the image boundary. So first the original images are padded to $1680 \times 1120$ with $(255, 255, 255)$, then open-source image-labeling tool roLabelImg\footnote{https://github.com/cgvict/roLabelImg} is utilized for raw annotation and finally, raw labels are processed by python scripts. The ROBB definition in Cattle-ROBB is illustrated in \cref{fig2a}, $(x_c, y_c)$ is the center point, $w$ and $h$ refer to along-body and cross-body dimension respectively, $\theta$ represents orientation in the range $[0, 2\pi)$ and counter-clockwise rotation starting from x-axis makes positive $\theta$. \cref{fig2b}, \cref{fig2c}, and \cref{fig2d} depict one, two, and three ROBBs in a single image respectively. 
