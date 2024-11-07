#!/bin/bash

# Loop through all folders in the current directory
for dir in */; do
    # Check if it's indeed a directory
    if [ -d "$dir" ]; then
        # Change to the current directory (subfolder)
        cd "$dir" || continue

        # Find the first .onnx or .tflite file
        model_file=$(find . -maxdepth 1 -type f \( -name "*.onnx" -o -name "*.tflite" \) | head -n 1)

        # If a model file exists
        if [ -n "$model_file" ]; then
            # Get the prefix of the model file (filename without extension and path)
            prefix="${model_file##*/}"
            prefix="${prefix%.*}"

            # Set the name of the new folder based on the prefix of the model file
            new_folder="../${prefix}"

            # Create the new folder
            mkdir -p "$new_folder"

            # Set the output .txt file name and path in the new folder
            txt_file="$new_folder/${prefix}.txt"

            # Create and write to the .txt file
            {
                echo "[network]"
                # Find and copy .tvn files to the new folder and list them in the .txt file with the required path
                for tvn_file in *.tvn; do
                    if [ -f "$tvn_file" ]; then
                        cp "$tvn_file" "$new_folder/"
                        echo "/opt/media/USBDriveA1/roseL-inference/${prefix}/$tvn_file"
                    fi
                done

                # Handle input files
                echo "[input]"
                for input_file in input.*.tv2b; do
                    if [ -f "$input_file" ]; then
                        cp "$input_file" "$new_folder/"
                        echo "/opt/media/USBDriveA1/roseL-inference/${prefix}/$input_file"
                    fi
                done

                # Handle golden files
                echo "[golden]"
                for output_file in output.*.tv2b; do
                    if [ -f "$output_file" ]; then
                        cp "$output_file" "$new_folder/"
                        echo "/opt/media/USBDriveA1/roseL-inference/${prefix}/$output_file"
                    fi
                done

                # Handle output data
                echo "[output]"
                echo "/opt/media/USBDriveA1/roseL-inference/${prefix}/output0.dat"
            } > "$txt_file"
        fi

        # Return to the parent directory
        cd ..
    fi
done

echo "Script completed."
