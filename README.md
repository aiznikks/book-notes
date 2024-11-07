#!/bin/bash

# Find all directories and run onecc -C config.cfg in each
for dir in */; do
  if [[ -f "$dir/config.cfg" ]]; then
    echo "Running onecc -C config.cfg in $dir"
    (cd "$dir" && onecc -C config.cfg)
  else
    echo "No config.cfg found in $dir, skipping..."
  fi
done
