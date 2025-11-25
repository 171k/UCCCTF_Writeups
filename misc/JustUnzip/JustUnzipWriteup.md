# Just Unzip Writeup

A really simple challenge to unzip 1000 times!

> #### Description
> 
> Unzip and unzip...
> 
> [zip file](https://ucc-ctf.noorraihan.com/files/98716f704a3c3d626f829991cdfb7b87/1000more.zip?token=eyJ1c2VyX2lkIjo4NywidGVhbV9pZCI6bnVsbCwiZmlsZV9pZCI6MTh9.aSVXrw.3pZ8eMmZ4hXlEDurJf38riVOMYc)



Okay so all we gotta do is craft a python script to unzip 1000 times.

This is the script i use by chatGPT:

```python
import zipfile
import os

# Path to the initial zip file
zip_path = "yourfile.zip"

# Directory to extract into
output_dir = "unzipped"
os.makedirs(output_dir, exist_ok=True)

iteration = 1

while True:
    with zipfile.ZipFile(zip_path, 'r') as zip_ref:
        # Get list of files in the zip
        namelist = zip_ref.namelist()
        if len(namelist) != 1:
            print(f"Multiple files found at iteration {iteration}, stopping.")
            break

        # Extract the single file
        extracted_file = namelist[0]
        zip_ref.extract(extracted_file, output_dir)
        print(f"Iteration {iteration}: extracted {extracted_file}")

    # Check if the extracted file is another zip
    next_zip_path = os.path.join(output_dir, extracted_file)
    if zipfile.is_zipfile(next_zip_path):
        # Prepare for next iteration
        zip_path = next_zip_path
        iteration += 1
    else:
        print(f"Final file reached: {next_zip_path}")
        break

```

and.. thats it, the zip will be unzipped 1000 times and you can now retrieve the flag!




