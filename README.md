#!/bin/bash

# Find all config.cfg files in subdirectories of the current directory
find . -type f -name "config.cfg" | while read -r config_file; do
    echo "Processing $config_file..."
    
    # Use awk to correctly identify the [one-quantize] section
    awk '
    BEGIN { in_section=0; added=0; }

    # If we encounter [one-quantize], enable the section flag
    /^\[one-quantize\]/ { in_section=1; }

    # If we encounter another section, disable the section flag
    /^\[.*\]/ && in_section { in_section=0; }

    # Add quantized_dtype=uint8 only after the output_path inside [one-quantize]
    in_section && /output_path/ && !added { 
        print $0;
        print "quantized_dtype=uint8";
        added=1;
        next;
    }

    # Print all lines
    { print $0; }
    ' "$config_file" > temp.cfg && mv temp.cfg "$config_file"
    
    echo "Updated $config_file"
done
