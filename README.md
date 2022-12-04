# 🎄 2022-advent-of-code-ojs

Local Quarto backup of the [Observable collection](https://observablehq.com/collection/@hrbrmstr/2022-advent-of-code) using:

```
#!/bin/bash
# Turn an Observable Notebook Collection into a directory of Quarto projects
#
# Provide the ""@user/collection" as the only command line argument and
# this script will create directories below the current directory filled with 
# Quarto projects made with ohq2quarto
#
# System Requirements:
#   - htmlq      # https://github.com/mgdm/htmlq
#   - jq         # https://stedolan.github.io/jq/
#   - ohq2quarto # https://github.com/hrbrmstr/ohq2quarto

if [ -z "$1" ]
then  
  echo "Usage: col2quarto.sh @user/collection"
	exit
fi

curl --silent "https://observablehq.com/collection/$1" | \
  htmlq --text "script#__NEXT_DATA__" | \
	jq -r '.props.pageProps.collection.listings.results[] | "https://observablehq.com/@" + (.owner.login) + "/" + (.slug) ' | \
	while read -r OBS_URL
	do
	  SLUG="$(basename "${OBS_URL}")"
		ohq2quarto --ohq-ref "${OBS_URL}" --output-dir "${SLUG}"

	done
```
