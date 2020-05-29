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

# cron entry for flying-m hash based monitoring
*/1 * * * * sleep 30 && /usr/local/bin/flym yourwebsite.com 7d3b369342b78e36e8d2cf30c619602c32ff6152f1bb24e443a0021a0677440d

Where that hash value of 7d3b369342b78e36e8d2cf30c619602c32ff6152f1bb24e443a0021a0677440d comes from this:

curl https://yourwebsite.complace | sha256sum

Or write the response to disk then create the hash like the script does:

curl https://yourwebsite.complace > hash.out
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
