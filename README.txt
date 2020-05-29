# flying-m
Hash based response monitoring.

###################################################################
#                                                                 #
#  .d888 888          d8b                                         #                     
# d88P"  888          Y8P                                         # 
# 888    888                                                      # 
# 888888 888 888  888 888 88888b.   .d88b.         88888b.d88b.   # 
# 888    888 888  888 888 888 "88b d88P"88b        888 "888 "88b  #
# 888    888 888  888 888 888  888 888  888 888888 888  888  888  # 
# 888    888 Y88b 888 888 888  888 Y88b 888        888  888  888  # 
# 888    888  "Y88888 888 888  888  "Y88888        888  888  888  # 
#                 888                   888                       #
#            Y8b d88P              Y8b d88P                       # 
#             "Y88P"                "Y88P"                        #
#                                                                 #
#                                                                 #
###################################################################         

Example usage:

# cron entry for flying-m hash based monitoring, monitoring github.com as an example:
*/1 * * * * sleep 30 && /usr/local/bin/flym github.com 8aff2234679933a4591ec1121f2066af0b2bb4f28f6cfaa0579731d1eed09a0a

Where that hash value of 8aff2234679933a4591ec1121f2066af0b2bb4f28f6cfaa0579731d1eed09a0a comes from this:

curl https://github.com | sha256sum

Or write the response to disk then create the hash like the script does:

curl https://github.com > hash.out
sha256sum hash.out


Then use SEC (https://simple-evcorr.github.io/) with the flym-sec.cfg and input set as your flym log files 
in $workdir/monitor/"$site".log

Updating the flym-sec.cfg to have the email address to send alerts for change to.

Call arbitrary scripts in response to any change. All changes to the index and access to the index will be detected because the output will have a unique hash, whether that is a network outage or a new line break in the index.html.

Idea: Don't send the alert emails, but instead spawn scripts that do advanced network and change recon,
and then those programs alert or email if they find an issue.

Warning: For every change that stays, you will need to update the crontab entry with the new "normal" hash.
If you run SEC and react to changes, you will be continually reacting/alerting until the hash in the crontab is
updated with the expected response or the expected response continues. The SEC config throttles emails
for this reason in the default with only one reactive script per 10 minutes etc.

The program is written to detect and use sha256 (BSD program) and fall back to sha256sum if sha256 is not found.
You can replace these with md5 and md5sum if you want to etc.
