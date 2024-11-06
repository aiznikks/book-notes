#!/bin/bash

# Loop through all folders in the current directory
for dir in */; do
    # Check if it's indeed a directory
    if [ -d "$dir" ]; then
        # Set the output .txt filename based on the folder name
        txt_file="${dir%/}.txt"

        # Create the .txt file and add initial content
        echo "[network]" > "$txt_file"

        # Change to the directory
        cd "$dir" || continue

        # Find any .tvn files and add them to the [network] section
        for tvn_file in *.tvn; do
            if [ -f "$tvn_file" ]; then
                echo "$tvn_file" >> "../$txt_file"
            fi
        done

        # Add the other fixed sections to the .txt file
        echo "[input]" >> "../$txt_file"
        echo "input.0.tv2b" >> "../$txt_file"
        echo "[golden]" >> "../$txt_file"
        echo "output.0.tv2b" >> "../$txt_file"
        echo "[output]" >> "../$txt_file"
        echo "output0.dat" >> "../$txt_file"

        # Return to the parent directory
        cd ..
    fi
done

echo "Script completed."
