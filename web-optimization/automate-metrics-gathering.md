## Automation Tooling

Below are some useful
* nodejs
* npm
* grunt

* protractor

## Install Google PageSpeed cli via npm

Install nodejs and npm, then run the following command to create the testing folder as well as install psi

<pre>
npm install --global psi
</pre>

"psi" is a command line tool for Google PageSpeed Insight. After it is installed, it can be run like the following

<pre>
psi https://www.wazyn.com --strategy desktop --threshold 50
</pre>

Where "50" is the threshold of score what the https://www.wazyn.com tries to meet, run the following command to see the 
error level (e.g. no. of times the threshold is not met)

<pre>
echo %errorlevel%
</pre>

## Install "Too Many Images" cli via npm

Run the following command to install tmi (too many images) cli:

<pre>
npm install --global tmi
</pre>

Run the following command to get statistics from tmi:

<pre>
tmi https://www.wazyn.com --strategy desktop --threshold 90
</pre>

Where "90" again is the threshold of the score set for the page to load

## Install Grunt

Run the following command to install grunt

<pre>
npm install --global grunt-cli
npm install --save-dev grunt
npm install --save-dev grunt-perfbudget
npm install --save-dev grunt-phantomas
</pre>
