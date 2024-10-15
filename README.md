#!/bin/bash

# Loop through each directory in the current folder (basic2)
for dir in */; do
    # Ensure it's a directory
    if [ -d "$dir" ]; then
        echo "Processing directory: $dir"
        
        # Check if config.cfg exists in the folder
        if [ -f "$dir/config.cfg" ]; then
            # Remove the leading '#' from lines in config.cfg
            sed -i 's/^#//' "$dir/config.cfg"
            
            echo "Uncommented config.cfg in $dir"
        else
            echo "No config.cfg found in $dir"
        fi
    fi
done

echo "Task completed."
