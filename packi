#!/bin/bash

# Get the number of repositories to scan from the user
echo "Enter the number of GitHub repositories to scan (up to 10):"
read num_repos

# Check that the number is within the valid range
if [ $num_repos -gt 10 ]; then
  num_repos=10
fi

# Loop through the number of repositories
for i in $(seq 1 $num_repos); do
  # Get the repository URL from the user
  echo "Enter the GitHub repository URL $i:"
  read repo_url

  # Clone the repository
  git clone $repo_url

  # Get the name of the repository
  repo_name=$(echo $repo_url | awk -F "/" '{print $NF}' | sed 's/.git//')

  # Change the working directory to the repository
  cd $repo_name

  # Find all of the package files in the repository
  packages=$(find . -name "*.yml" -o -name "*.yaml" -o -name "requirements*.txt" -o -name "*.json" -o -name "*.lock")

  # Check if the repository is private or public
  repo_type=$(curl -I $repo_url | grep "Status" | awk '{print $2}')
  if [ $repo_type == "200" ]; then
    # The repository is public, so save the packages to public.txt
    echo "Packages found in public repository $i ($repo_name):" >> ../public.txt
    for package in $packages; do
      echo " - $package" >> ../public.txt
    done
  else
    # The repository is private, so save the packages to private.txt
    echo "Packages found in private repository $i ($repo_name):" >> ../private.txt
    for package in $packages; do
      echo " - $package" >> ../private.txt
    done
  fi

  # Go back to the parent directory
  cd ..
done

echo "Private packages have been saved to private.txt"
echo "Public packages have been saved to public.txt"
