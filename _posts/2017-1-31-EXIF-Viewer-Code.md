# EXIF Viewer Code

This is my C utility code as required for a class assignment.  I have not altered my code since submission in February 2017.
```C
/********************
John Dott
CS 449: Project 1 Part 2
Exif Viewer
Due 2/5/2017
*********************/
#include <stdio.h>
#include <string.h>
int main(int argc, char *argv[]){
	/*define struct for te first 20 bytes of image */
	struct first_tag {
		char start_marker[2];
		char APP1_marker[2];
		char APP1[2];
		char exif_string[4];
		char nul_zero[2];
		char endian[2];
		char version_num[2];
		int offset;
	};
	
	struct TIFF_tag {
		unsigned short ident;
		unsigned short type;
		int num_items;
		int val_offset;
	};
	
	/* declare the structs needed */
	struct first_tag tag;
	struct TIFF_tag temp_tiff;
	
	/* declare identifiers for TIFF tags that we care about as constants */
	const unsigned short man_string			= 0x010F;
	const unsigned short cam_mod			= 0x0110;
	const unsigned short sub_block_address 	= 0x8769;
	const unsigned short width_ident 		= 0xA002;
	const unsigned short height_ident 		= 0xA003;
	const unsigned short iso 				= 0x8827;
	const unsigned short exposure_ident 	= 0x829A;
	const unsigned short fstop_ident 		= 0x829D;
	const unsigned short focal_ident 		= 0x920A;
	const unsigned short date_ident 		= 0x9003;
	
	/* Define program variables */
	unsigned short temp_ident, count;
	int i, j;
	char letter;
	char manu_str[50], cam_mod_str[50];
	long current_offset;
	int height, width;
	short iso_speed;
	unsigned int expose_speed[2];
	unsigned int fstop[2];
	unsigned int focal_length[2];
	char date_taken[50];
	char exif[4], unsupported_endian[2];
	
	strncpy(exif, "Exif", 4);
	strncpy(unsupported_endian, "MM", 2);
	
	/* open file*/
	FILE *f = fopen(argv[1], "rb");
	if (f == NULL){
		printf("ERROR READING INPUT FILE.  Terminating program.\n\n");
		return 1;
	}
	
	 /*read first tag of .JPG file and get count */
	fread(&tag, sizeof(tag), 1, f);
	
	/* terminate if exif tag non-existant or wrong endianness */
	if(strcmp(tag.exif_string,exif) != 0){
		printf("Exif tag not found, terminating program.\n");
		return 1;
	} else if (strcmp(tag.endian, unsupported_endian) == 0){
		printf("Endianness not supported.\n");
		return 1;
	}
		
	/* get count */
	fread(&count, sizeof(unsigned short), 1, f);
	
	/*loop through and get TIFF tags we care about*/
	for(i = 0; i < count; i++){
		fread(&temp_tiff, sizeof(temp_tiff), 1, f);
		current_offset = ftell(f);
		
		temp_ident = temp_tiff.ident;
	
		if(temp_ident == man_string){
			/* Seek to file position 12 + offset */
			fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
			
			/*read each letter until NUL termintor */
			j=0;
			do {
				fread(&letter, sizeof(char), 1, f);
				manu_str[j] = letter;
				j++;
			} while(letter != '\0');
			/* created full string, seek back to end of tag to contine */
			fseek(f, current_offset, SEEK_SET);
			
		} else if(temp_ident == cam_mod){
			/* Seek to file position 12 + offset */
			fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
			
			/*read each letter until NUL termintor */
			j=0;
			do {
				fread(&letter, sizeof(char), 1, f);
				cam_mod_str[j] = letter;
				j++;
			} while(letter != '\0');
			/* created full string, seek back to end of tag to contine */
			fseek(f, current_offset, SEEK_SET);
			
		} else if(temp_ident == sub_block_address){
			fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
			fread(&count, sizeof(unsigned short), 1, f);
			for (i = 0; i < count; i++){
				fread(&temp_tiff, sizeof(temp_tiff),1,f);
				temp_ident = temp_tiff.ident;
				current_offset = ftell(f);
				
				/* get information from tags we care about */
				if ( temp_ident == width_ident){
					width = temp_tiff.val_offset;
				} else if (temp_ident == height_ident){
					height = temp_tiff.val_offset;
				} else if (temp_ident == iso){
					iso_speed = temp_tiff.val_offset;
				} else if (temp_ident == exposure_ident){
					fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
					fread(&expose_speed[0], sizeof(unsigned int), 1, f);
					fread(&expose_speed[1], sizeof(unsigned int), 1, f);
					fseek(f, current_offset, SEEK_SET);
				} else if (temp_ident == fstop_ident){
					fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
					fread(&fstop[0], sizeof(unsigned int), 1, f);
					fread(&fstop[1], sizeof(unsigned int), 1, f);
					fseek(f, current_offset, SEEK_SET);
				} else if (temp_ident == focal_ident){
					fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
					fread(&focal_length[0], sizeof(unsigned int), 1, f);
					fread(&focal_length[1], sizeof(unsigned int), 1, f);
					fseek(f, current_offset, SEEK_SET);
				} else if (temp_ident == date_ident){
					/* Seek to file position 12 + offset */
					fseek(f, 12 + temp_tiff.val_offset, SEEK_SET);
					
					/*read each letter until NUL termintor */
					j=0;
					do {
						fread(&letter, sizeof(char), 1, f);
						date_taken[j] = letter;
						j++;
					} while(letter != '\0');
					/* created full string, seek back to end of tag to contine */
					fseek(f, current_offset, SEEK_SET);
				}
				
			}
			
			/* no longer need to continue reading TIFF tags */
			break;
		}
	}
	/* Print all relevant information */
	printf("Manufacturer:\t%s\n", manu_str);
	printf("Model: 		%s\n", cam_mod_str);
	printf("Exposure Time:\t%u/%u second\n", expose_speed[0],expose_speed[1]);
	printf("F-stop:\t\tf/%2.1f\n", ((float)fstop[0]/fstop[1]));
	printf("ISO:		ISO %hd\n", iso_speed);
	printf("Date Taken:	%s\n", date_taken);
	printf("Focal Length:\t%-3.0f mm\n", ((float)focal_length[0]/focal_length[1]));
	printf("Width: 		%d pixels\n", width);
	printf("Height:		%d pixels\n", height);
	return 0;
}
```
