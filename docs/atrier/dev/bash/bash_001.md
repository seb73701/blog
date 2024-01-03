---
title: Analyse des arguments de ligne de commande et attribution de valeurs
description: 
published: true
date: 2023-04-21T11:00:32.194Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:00:27.116Z
---

Developing scripts for a big data workflow and feeling the pain of using positional parameters? Not to worry. Read on to learn about using named parameters in bash.

While developing scripts for a big data workflow, I was required to develop a master and child script execution scenario. In the master as well as the child script, we were passing parameters. Given my Java background and the fact that I use a self-developed command-line argument parser, I was feeling the pain of using positional parameters.

Fortunately, using named parameters is an easy task in bash, as well. Here is a snippet:

<code>. . .
while [[ $# -gt 0 ]]
do
    key="${1}"
    case ${key} in
    -i|--input)
        INPUTPARAM="${2}"
        shift # past argument
        shift # past value
        ;;
    -o|--output)
        OUTPUTPARAM="${2}"
        shift # past argument
        shift # past value
        ;;
    -h|--help)
        echo "Show help"
        shift # past argument
        ;;
    *)    # unknown option
        shift # past argument
        ;;
    esac
    shift
done
. . .

</code>

Reading command-line arguments is one part of the story. The second part is where we need to check the parameter for ''null'' and either assign a default value or display an error. I accomplished this using a helper function, as shown below:

<code>
. . .
PADDING_TEXT="padding"
parameterValue() {
    if [[ "${1} ${PADDING_TEXT}" == " ${PADDING_TEXT}" ]]; then
        VALUE=${2}
    else
        VALUE=${1}
    fi
    echo ${VALUE}
}
. . .
. . .
DEFAULT_INPUT_PATH="/someDirectory"
INPUT_PATH=$(parameterValue ${INPUTPARAM} ${DEFAULT_INPUT_PATH})
. . .
</code>

**Correction:** In my [[https://dzone.com/articles/bash-snippet-reading-values-from-a-configuration-f|previous article]], I described a function to read values from a configuration file. The function has an error: the last line of the function namely an echo command is missing. Hence, the correct snippet is:

<code>
getValueFromConfig() {
    VALUE=`grep ${1} config.sys | cut -d '=' -f 2`
    echo ${VALUE} ###<< missing statement
}
</code>

And that's it!

Source : [[https://dzone.com/articles/bash-snippet-parsing-command-line-arguments-and-as]]