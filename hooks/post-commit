#!/bin/bash

# This is a custom git hook installed for codespaces when you'll be unable to push your changes to the source repository.
# Instead, we'll fork the repo on your behalf, ensure you have appropriate permissions, and transfer the codespace to that fork.

trap "mv $0 $0.codespaces-disabled" EXIT

echo "You don't have write access to the $GITHUB_REPOSITORY repository, so you cannot push changes to it."
echo "To obtain write access we will point this codespace at your fork of $GITHUB_REPOSITORY, creating that fork if it doesn't exist."
echo

if [ -t 1 ]; then
	read -p "Would you like to proceed? " -n 1 -r < /dev/tty
	echo
	if [[ ! $REPLY =~ ^[Yy]$ ]]
	then
		exit 0
	fi
fi

URL="https://api.github.com/vscs_internal/user/$GITHUB_USER/codespaces/$CODESPACE_NAME/fork_repo"


current_branch=$(git rev-parse --abbrev-ref HEAD)

response=$(curl -s -w "%{http_code}" -X POST -H "Authorization: token $GITHUB_TOKEN" -d "{\"branch\":\"$current_branch\"}" $URL)

# Grab the last line which is the status code
http_code=$(tail -n1 <<< "$response")

while [ "$http_code" != "200" ]; do
  echo "Whoops, we weren't able to set up write access for you. Request failed with status $http_code."
  echo
	if [ -t 1 ]
	then
		read -p "Retry? " -n 1 -r < /dev/tty
		echo
		if [[ ! $REPLY =~ ^[Yy]$ ]]
		then
			exit 0
		fi

		response=$(curl -s -w "%{http_code}" -X POST -H "Authorization: token $GITHUB_TOKEN" $URL)
		http_code=$(tail -n1 <<< "$response")
	else
		exit 0
	fi
done

# Grab everything _but_ the last line, which is the body
content=$(sed '$ d' <<< "$response")
# Parse the body into necessary git context
clone_url=$(jq -r .repository.clone_url <<< "$content")
ref=$(jq -r .ref <<< "$content")

# Unset the remote on the current branch so that the next push will prompt the user to pick the remote to push to
git branch --unset-upstream $current_branch

# Reconfigure remotes
git remote rename origin upstream
git remote add origin $clone_url

# Fetch the new fork and reset the upstream branch. Note that this is best effort and might fail if the fork takes too long to be created.
git fetch origin $ref
git branch --set-upstream-to origin/$ref $current_branch

echo
echo "We've configured write access for you! Use the remote 'origin' to interact with your fork. Use the remote 'upstream' to interact with the parent repository."
exit 0
