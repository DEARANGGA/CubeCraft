CubeCraft
=========
#include <GL/gl.h>
#include <GL/glu.h>
#include <GL/glut.h>
#include <GL/glaux.h>  	//Used for loading the textures
#include <windows.h>
#include <gl/glext.h>

#include <stdio.h>

#include <iostream>
#include <string.h>

using namespace std;

float object01_limits[6];
float object01_center[3];
float object01_size;
int Animated = 0;
int Wireframe = 0;
GLfloat rotLx = 0.0f; 
GLfloat rotLy = 0.0f;
GLfloat rotLz = 0.0f;
static GLfloat xRot = 0.0f;
static GLfloat yRot = 0.0f;

char path[50];
int pointerPath = 0;

//Background
GLuint bg1[2];  //belakang
GLuint bg2[2]; //samping kanan & kiri
GLuint bg3[2]; //lantai

//Kepala
GLuint texturekepala1[2]; //Muka
GLuint texturekepala2[2]; //Samping Kanan
GLuint texturekepala3[2]; //Belakang
GLuint texturekepala4[2]; //Samping Kiri
GLuint texturekepala5[2]; //Atas
GLuint texturekepala6[2]; //Bawah

//Kepala Yui
GLuint texturekepalaYui1[2]; //Muka
GLuint texturekepalaYui2[2]; //Samping Kanan
GLuint texturekepalaYui3[2]; //Belakang
GLuint texturekepalaYui4[2]; //Samping Kiri
GLuint texturekepalaYui5[2]; //Atas

//Kepala Mugi
GLuint texturekepalaMugi1[2]; //Muka
GLuint texturekepalaMugi2[2]; //Samping Kanan
GLuint texturekepalaMugi3[2]; //Belakang
GLuint texturekepalaMugi4[2]; //Samping Kiri
GLuint texturekepalaMugi5[2]; //Atas

//Badan Mio
GLuint texturebadan1[2]; //Depan
GLuint texturebadan2[2]; //Kanan
GLuint texturebadan3[2]; //Belakang
GLuint texturebadan4[2]; //Kiri
GLuint texturebadan5[2]; //Bawah

//Badan Yui
GLuint texturebadanYui1[2]; //Depan
GLuint texturebadanYui2[2]; //Belakang
GLuint texturebadanYui3[2]; //Bawah

//Badan Mugi
GLuint texturebadanMugi1[2]; //Depan
GLuint texturebadanMugi2[2]; //Kanan
GLuint texturebadanMugi3[2]; //Belakang

//Rok
GLuint textureRok[2]; //Depan

//Kaki
GLuint textureKaki1[2]; //Depan
GLuint textureKaki2[2]; //Samping
GLuint textureKaki3[2]; //Belakang

//Kaki Yui
GLuint textureKakiYui1[2]; //Depan
GLuint textureKakiYui2[2]; //Samping
GLuint textureKakiYui3[2]; //Belakang

//Kaki Mugi
GLuint textureKakiMugi1[2]; //Depan
GLuint textureKakiMugi2[2]; //Samping
GLuint textureKakiMugi3[2]; //Belakang

//Tangan Kanan
GLuint textureTangan1[2]; //Atas
GLuint textureTangan2[2]; //Kanan
GLuint textureTangan3[2]; //Bawah
GLuint textureTangan4[2]; //Bahu
GLuint textureTangan5[2]; //Jari

//Tangan Kiri
GLuint textureTangan7[2]; //Atas
GLuint textureTangan8[2]; //Kanan
GLuint textureTangan9[2]; //Bawah
GLuint textureTangan10[2]; //depan
GLuint textureTangan11[2]; //belakang

//Background
/*Image Belakang
  Image2 Samping Kanan & Kiri
  Image3 Lantai
*/
struct Background {
unsigned long sizeX;
unsigned long sizeY;
char *data;
};

//Kepala
/*Kepala1 Muka
  Kepala2 Samping Kanan
  Kepala3 Belakang
  Kepala4 Samping Kiri
  Kepala5 Atas
  Kepala6 Bawah
*/
struct Kepala {
unsigned long sizeX;
unsigned long sizeY;
char *data;
};

//Badan
/*Badan1 Depan
  Badan2 Samping Kanan
  Badan3 Belakang
  Badan4 Samping Kiri
  Badan5 Bawah
*/
struct Badan {
unsigned long sizeX;
unsigned long sizeY;
char *data;
};

//Rok
struct Rok {
unsigned long sizeX;
unsigned long sizeY;
char *data;
};

//Kaki
/*Kaki1 Depan
  Kaki2 Samping
  Kaki3 Belakang
*/
struct Kaki {
unsigned long sizeX;
unsigned long sizeY;
char *data;
};

//Tangan
/*Tangan1 Depan
  Tangan2 Samping
  Tangan3 Belakang
  Tangan4
  Tangan5
*/
struct Tangan {
unsigned long sizeX;
unsigned long sizeY;
char *data;
};

//Background
typedef struct Background Image,Image2,Image3;

//Kepala
typedef struct Kepala Kepala1,Kepala2,Kepala3,Kepala4,Kepala5,Kepala6,KepalaMugi1,KepalaMugi2,KepalaMugi3,KepalaMugi4,KepalaMugi5,KepalaMugi6,KepalaYui1,KepalaYui2,KepalaYui3,KepalaYui4,KepalaYui5;

//Badan
typedef struct Badan Badan1,Badan2,Badan3,Badan4,Badan5,BadanMugi1,BadanMugi2,BadanMugi3,BadanYui1,BadanYui2,BadanYui3,;

//Rok
typedef struct Rok Rok;

//Kaki
typedef struct Kaki Kaki1, Kaki2, Kaki3, KakiMugi1, KakiMugi2, KakiMugi3, KakiYui1, KakiYui2, KakiYui3;

//Tangan Kanan
typedef struct Tangan Tangan1, Tangan2, Tangan3, Tangan4, Tangan5;

//Tangan Kiri
typedef struct Tangan Tangan7, Tangan8, Tangan9, Tangan10, Tangan11;

#define checkImageWidth 600
#define checkImageHeight 600

GLubyte checkImage[checkImageWidth][checkImageHeight][3];

void makeCheckImage(void) {
    int i, j, c;
    for (i = 0; i < checkImageWidth; i++) {
        for (j = 0; j < checkImageHeight; j++) {
            c = ((((i&0x8)==0)^((j&0x8)==0)))*255;
            checkImage[i][j][0] = (GLubyte) c;
            checkImage[i][j][1] = (GLubyte) c;
            checkImage[i][j][2] = (GLubyte) c;
        }
    }
}

//[Image1]
int ImageLoad(char *filename, Image *image) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&image->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&image->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = image->sizeX * image->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    image->data = (char *) malloc(size);
    if (image->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(image->data, size, 1, file)) != 1) {
    printf("Error reading image data from %s.\n", filename);
    return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = image->data[i];
        image->data[i] = image->data[i+2];
        image->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Image2]
int ImageLoad2 (char *filename, Image2 *image2) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&image2->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&image2->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = image2->sizeX * image2->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    image2->data = (char *) malloc(size);
    if (image2->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(image2->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = image2->data[i];
        image2->data[i] = image2->data[i+2];
        image2->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Image3]
int ImageLoad3 (char *filename, Image3 *image3) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&image3->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&image3->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = image3->sizeX * image3->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    image3->data = (char *) malloc(size);
    if (image3->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(image3->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = image3->data[i];
        image3->data[i] = image3->data[i+2];
        image3->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Kepala1] Muka
int ImageLoadKepala1 (char *filename, Kepala1 *kepala1) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&kepala1->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&kepala1->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = kepala1->sizeX * kepala1->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    kepala1->data = (char *) malloc(size);
    if (kepala1->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(kepala1->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = kepala1->data[i];
        kepala1->data[i] = kepala1->data[i+2];
        kepala1->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Kepala2] Samping Kanan
int ImageLoadKepala2 (char *filename, Kepala2 *kepala2) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&kepala2->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&kepala2->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = kepala2->sizeX * kepala2->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    kepala2->data = (char *) malloc(size);
    if (kepala2->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(kepala2->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = kepala2->data[i];
        kepala2->data[i] = kepala2->data[i+2];
        kepala2->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Kepala3] Belakang
int ImageLoadKepala3 (char *filename, Kepala3 *kepala3) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&kepala3->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&kepala3->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = kepala3->sizeX * kepala3->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    kepala3->data = (char *) malloc(size);
    if (kepala3->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(kepala3->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = kepala3->data[i];
        kepala3->data[i] = kepala3->data[i+2];
        kepala3->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Kepala4] Samping Kiri
int ImageLoadKepala4 (char *filename, Kepala4 *kepala4) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&kepala4->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&kepala4->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = kepala4->sizeX * kepala4->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    kepala4->data = (char *) malloc(size);
    if (kepala4->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(kepala4->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = kepala4->data[i];
        kepala4->data[i] = kepala4->data[i+2];
        kepala4->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Kepala5] Atas
int ImageLoadKepala5 (char *filename, Kepala5 *kepala5) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&kepala5->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&kepala5->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = kepala5->sizeX * kepala5->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    kepala5->data = (char *) malloc(size);
    if (kepala5->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(kepala5->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = kepala5->data[i];
        kepala5->data[i] = kepala5->data[i+2];
        kepala5->data[i+2] = temp;
    }
    // we're done.
    return 1;
}

//[Kepala6] Bawah
int ImageLoadKepala6 (char *filename, Kepala6 *kepala6) {
    FILE *file;
    unsigned long size; // size of the image in bytes.
    unsigned long i; // standard counter.
    unsigned short int planes; // number of planes in image (must be 1)
    unsigned short int bpp; // number of bits per pixel (must be 24)
    char temp; // temporary color storage for bgr-rgb conversion.
    // make sure the file is there.
    if ((file = fopen(filename, "rb"))==NULL) {
        printf("File Not Found : %s\n",filename);
        return 0;
    }
    // seek through the bmp header, up to the width/height:
    fseek(file, 18, SEEK_CUR);
    // read the width
    if ((i = fread(&kepala6->sizeX, 4, 1, file)) != 1) {
        printf("Error reading width from %s.\n", filename);
        return 0;
    }
    
    //printf("Width of %s: %lu\n", filename, image->sizeX);
    
    // read the height
    if ((i = fread(&kepala6->sizeY, 4, 1, file)) != 1) {
        printf("Error reading height from %s.\n", filename);
        return 0;
    }
    
    //printf("Height of %s: %lu\n", filename, image->sizeY);
    // calculate the size (assuming 24 bits or 3 bytes per pixel).
    size = kepala6->sizeX * kepala6->sizeY * 3;
    // read the planes
    if ((fread(&planes, 2, 1, file)) != 1) {
        printf("Error reading planes from %s.\n", filename);
        return 0;
    }
    if (planes != 1) {
        printf("Planes from %s is not 1: %u\n", filename, planes);
        return 0;
    }
    
    // read the bitsperpixel
    
    if ((i = fread(&bpp, 2, 1, file)) != 1) {
        printf("Error reading bpp from %s.\n", filename);
        return 0;
    }
    
    if (bpp != 24) {
        printf("Bpp from %s is not 24: %u\n", filename, bpp);
        return 0;
    }
    
    // seek past the rest of the bitmap header.
    
    fseek(file, 24, SEEK_CUR);
    // read the data.
    kepala6->data = (char *) malloc(size);
    if (kepala6->data == NULL) {
        printf("Error allocating memory for color-corrected image data");
        return 0;
    }
    
    if ((i = fread(kepala6->data, size, 1, file)) != 1) {
        printf("Error reading image data from %s.\n", filename);
        return 0;
    }
    for (i=0;i<size;i+=3) { // reverse all of the colors. (bgr -> rgb)
        temp = kepala6->data[i];
        kepala6->data[i] = kepala6->data[i+2];
        kepala6->data[i+2] = temp;
    }
    // we're done.
    return 1;
}
