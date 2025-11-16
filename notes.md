# Personal Notes

### Importing data from google drive
from google.colab import drive
drive.mount('/content/drive')

### Reading images from drive
import cv2
cv2.imread(path_to_image)