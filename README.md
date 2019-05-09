# Python S3 Concat

[![PyPI](https://img.shields.io/pypi/v/s3-concat.svg)](https://pypi.python.org/pypi/s3-concat)
[![PyPI](https://img.shields.io/pypi/l/s3-concat.svg)](https://pypi.python.org/pypi/s3-concat)  


S3 Concat is used to concatenate many small files in an s3 bucket into fewer larger files.


## Install
`pip install s3-concat`


## Usage

### Command Line
`$ s3-concat -h`

### Import
```python
bucket = 'YOUR_BUCKET_NAME'
out_path = 'FILE_TO_SAVE_TO'
min_file_size = '50MB'
path_to_concat = 'PATH_TO_FILES_TO_CONCAT'

# Init the job
job = S3Concat(bucket, out_path, min_file_size, content_type='application/json')
# Add files, can call multiple times to add files from other directories
job.add_files(path_to_concat)
# Only use small_parts_threads if you need to. See Advanced Usage section below.
job.concat(small_parts_threads=4)
```

## Advanced Usage

Depending on your use case, you may want to use `small_parts_threads`.  

  - `small_parts_threads` is only used when the files you are trying to concat are less then 5MB. Due to the limitations of the s3 multipart_upload api (see *Limitations* below) any files less then 5MB need to be download locally, concated together, then re uploaded. By setting this thread count it will download the parts in parallel for faster creation of the concatination process.  

The values set for these arguments depends on your use case and the system you are running this on.


## Limitations
This uses the multipart upload of s3 and its limits are https://docs.aws.amazon.com/AmazonS3/latest/dev/qfacts.html
