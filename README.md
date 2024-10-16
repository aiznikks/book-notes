for file in *; do 
    if [ -f "$file" ]; then 
        prefix="${file%%.*}"               # Get the prefix of the file
        new_dir="${prefix}_model"          # Create the new directory name
        mkdir -p "$new_dir"                # Create the directory if it doesn't exist
        cp "$file" "$new_dir/"             # Copy the file to the new directory
    fi 
done
