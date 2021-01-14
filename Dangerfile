#!/usr/bin/env ruby

require 'active_support/core_ext/string/output_safety'

def verifyPRTitle()
    title = github.pr_title
    if title.size < 17
        warn("Title too short. It should fit '[KLTB002-JIRA_TICKET_NO] TITLE' template.")
    else 
        if title.start_with?("[KLTB002-")
            jiraTicketNumber = title[9, 5]
            trailingBracket = title[14, 2]
            warn "Title trailing bracket or space missing. It should fit '[KLTB002-JIRA_TICKET_NO] TITLE template'" unless trailingBracket == "] "
            warn "Invalid JIRA ticket number It should fit [KLTB002-JIRA_TICKET_NO] TITLE template'" unless jiraTicketNumber.scan(/\D/).empty?
            jiraTicketBaseString = "https://kolibree.atlassian.net/browse/KLTB002-#{jiraTicketNumber}"
            jiraTicket = jiraTicketBaseString.gsub(URI.regexp, '<a href="\0">\0</a>').html_safe
            message "This PR is related to JIRA " + jiraTicket
            description = github.pr_body
            warn "JIRA link in the description does not match pull request title." unless description.include?(jiraTicketBaseString)
        else
            warn "Invalid PR title. It should fit [KLTB002-JIRA_TICKET_NO] TITLE template"
        end
    end
end

verifyPRTitle()
