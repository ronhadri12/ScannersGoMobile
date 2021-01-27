# ScannersGoMobile
Mobile Scanner Project

Introduction
        Scanners play an important role for students as they are frequently used to provide students with copies of pages from a textbook that they couldn’t afford to buy, scan completed handwritten documents/assignments to submit online, etc. However, they are not easily accessible if you’re not near a library and they are not portable. Therefore, I have decided to implement an algorithm that, given a photo of a document, detects the borders of the paper and stretches the region enclosed by the borders, based on the “normalized” image size (which will be predetermined). This photo could be one that is taken on a cellphone or any other device that is capable of taking photos. In most cases, photos taken on a cellphone may be set at an angle or have some kind of rotation associated with them. My algorithm is designed to fix this issue. The algorithm will extract only the content on the paper of the document, ignoring noise outside of the borders of the paper. The content enclosed by the borders of the image will be stretched and considered as the scanned image. Then, a technique is applied to filter the scanned image and make the content more readable.

Algorithm Analysis
        The main objectives of the algorithm are to detect the corners of an image, map the corners to a new image matrix, and interpolate all the information contained within the corners from the original image. The algorithm is initiated by loading the original image and displaying it. To test my algorithm, nine images were taken of documents in various layout configurations. The configurations include: normal, clockwise rotation, right angle, top angle, far normal, bottom angle, left angle, and counterclockwise rotation. Seven of the nine images were taken of the document in these configurations in a dark background setting. Two of the nine images were taken in a light background setting to test the algorithm’s functionality in a different environment. The image was then converted into grayscale and double precision to have a more precise scaling of the various intensities in the image. Then a gaussian filter with a standard deviation of three is applied on the grayscale image to blur it. This technique is used to remove any noise that is present in the image and is a necessary step for correct identification of corners in later steps. The gradient magnitude and direction are calculated on the the blurred image using the Prewitt method. The resulting gradient magnitude is converted into binary and thresholded using a value of 0.1. The gradient magnitude of the blurred image and thresholded image are displayed to show the effects of those respective steps. A convex hull image is computed from the binary image and displayed along with an edge-detected version of the convex hull image using a derivative of the Canny method. The latter image is used to display the points detected by the Harris Stephens algorithm.
	Once the convex hull image has been computed, the Harris Stephens function is utilized to detect the corners of the image. The strongest twenty points are identified from the detected corners. A strongest point is defined as one having the strongest metrics when compared to another pixel. A new image is defined having the same size as the original image and initialized to a zero matrix. Another matrix is created signifying the corner points of the new image. This matrix of points will be input as the fixedPoints parameter of the mapping function.
	The other parameter for the mapping function is a matrix of points for the movingPoints, identified from the strongest points. Each of the twenty strongest points are passed into a sub-algorithm that identifies the x-coordinate and y-coordinate of the point closest to the four corners. This sub-algorithm is based on the four quadrants of the image. Once the point has been identified as being located in one of these quadrants, it is compared to all of the other points in that quadrant to find the approximate cornerpoint.
	Once the fixedPoints and the movingPoints have been identfied, an algorithm can be used to map the movingPoints to the fixedPoints. Then the content between the movingPoints is interpolated and mapped into the area between the fixedPoints. The result of this algorithm is the scanned image.
	Some of the resulting scanned images were a little difficult to read. Therefore, I utilized unsharped masking to sharpen the image. The result of this function is the final filtered image.

Results
        The pictures that I used to test my algorithm were taken by my phone, whose camera already filters noise from the images captured by it to a certain degree. However, they still display uneven illumination and the background is still apparent. Therefore, the images captured by my phone camera are the original images that are displayed. After interpolating the original image, I end up with a scanned image, which is just an upright, zoomed version of the original. Then, the last step in my algorithm sharpens the scanned image so that the content on the scanned image is more clear. The result is the final, filtered image. I have provided a screenshot of the original image compared to the filtered one, for each of the nine test images that I took. However, for the second image, I also decided to display the scanned image and compare that to the filtered one to detail the success of my sharpening technique that was used on all of the images.
