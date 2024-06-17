# Understanding SPF, DKIM, and DMARC: A Simple Guide

Email security is a key part of internet communication. But what are SPF, DKIM, and DMARC, and how do they work? This guide will explain it all in simple terms to make these concepts clearer.

## Table of Contents

1. [What Is This Guide For and Why Bother?](#what-is-this-guide-for-and-why-bother)
2. [Why Choose This Guide?](#why-choose-this-guide)
3. [What This Guide Is Not For](#what-this-guide-is-not-for)
4. [SPF, DKIM, and DMARC: Simplified](#spf-dkim-and-dmarc-simplified)
5. [Real World Examples of where SPF, DKIM, and DMARC are used](#real-world-examples-of-where-spf-dkim-and-dmarc-are-used)
6. [Now I Know These Things, What's Next?](#now-i-know-these-things-whats-next)
7. [Checking Your SPF, DKIM, DMARC Status](#checking-your-spf-dkim-dmarc-status)
8. [FAQ's with SPF, DKIM and DMARC](#faqs-with-spf-dkim-and-dmarc)
9. [Wrapping Up](#wrapping-up)
10. [Contributing](#contributing)
11. [Sharing is Caring](#sharing-is-caring)
12. [Contact](#contact)
13. [References](#references)

## What Is This Guide For and Why Bother?

If you are invovled in developing, supporting, or maintaining an application that sends emails, this guide is a must read. This guide is your key to peace of mind, knowing that your emails will reach your customers as intended and your domain is shielded from abuse from cybercriminals and spammers.

It's about ensuring they reach the intended destination - the recipient's inbox, not the spam or junk folder. For instance, You've built an e-commerce application or a SaaS platform that sends transactional emails like order confirmations or password resets or important customer notification emails. These emails are crucial touchpoints for your customers. But what if they never see them? What if these important communications end up in spam or junk? 

While email is one of the most common communication channels, it's also a favorite target for cybercriminals and a playground for spammers. Here are some real-world examples of how they can abuse email systems:

- **Phishing Attacks**: A cybercriminal wants to steal sensitive information from the customers of a well-known bank. The criminal could spin up or use a compromised server to send emails that appear to come from the bank's domain, asking customers to update their account information. If the bank hasn't implemented SPF, the email could pass the receiving server's checks and land in the customer's inbox. The customer, thinking the email is from their bank, clicks the link and enters their login details on a fake website controlled by the criminal. The criminal can now access the customer's bank account.

- **Brand Impersonation**: A cybercriminal could impersonate a popular e-commerce platform and send emails to users asking them to confirm their purchase of an expensive item. The email could contain a link to a fake customer support page where the user is asked to enter their login details to cancel the purchase. If the e-commerce platform hasn't implemented DKIM, the email could pass the receiving server's checks and land in the user's inbox. The user, thinking the email is from the e-commerce platform, enters their login details on the fake page, giving the criminal access to their account.

- **Business Email Compromise (BEC)**: A cybercriminal could impersonate a company's CEO or another high-ranking official and send an email to the finance department, asking them to make a payment to a new vendor. If the company hasn't implemented DMARC, the email could pass the receiving server's checks and land in the finance department's inbox. The finance department, thinking the email is from the CEO, could make the payment to the criminal's bank account.

By understanding and implementing SPF, DKIM, and DMARC, you can protect your domain from being used in these types of attacks, safeguard your customers and employees, and maintain your reputation.

So, why bother? Because your emails matter, your customers matter, and your reputation matters.

## Why Choose This Guide?

With so many articles out in the internet why should I choose this guide? This guide stands out for its simplicity, clarity, and convenience. It demystifies SPF, DKIM, and DMARC with clear explanations and examples avoiding the technical jargon as much as possible. Hosted on GitHub, it integrates seamlessly with your development environment, providing quick access to information right from your IDE (visual studio code ,etc.) or command line. Plus, it's a document that will stay in Github and guaranteed that won't go anywhere that can be edited by you or anyone or the community to ensure it stays updated and relevant.

## What This Guide Is Not For

While this guide aims to simplify SPF, DKIM, and DMARC, it's not intended to be a comprehensive guide about these topics. It's not a guide for setting up an email server, nor does it cover advanced topics like encryption or secure email gateways.

## SPF, DKIM, and DMARC: Simplified

### SPF (Sender Policy Framework)

SPF: It's like a list of friends who can send emails for you. The SPF Record is this list. If an email says it's from you but it's not sent by a friend on your list, it's probably not really from you.

As the owner of a domain, you can use SPF to create a list of 'email friends' - these are the mail servers that are allowed to send emails on your behalf. This helps stop people who aren't your 'email friends' from pretending to be you. The SPF Record, a DNS TXT record, is where you keep this list of 'email friends'.

The DNS TXT record for an SPF your 'email friends' typically looks like this:
```bash
v=spf1 ip4:123.123.123.123 ~all
```
Here's the command I usually run to fetch that:
```bash
dig TXT example.com
```

### DKIM (DomainKeys Identified Mail)

DKIM: It's like a secret note inside your emails. When you send an email, you put a secret note inside. This note is made using a special secret code only you know. 
When your email arrives, the receiver checks the secret note using a public code that everyone knows. This public code is stored in a place called the DKIM Record. 
If the secret note matches the public code, the email is really from you and hasn't been changed. This helps stop bad people from pretending to send emails from you or changing your emails.

This public code, also known as a public key, is stored in a DNS TXT record known as the DKIM Record, which is accessible to everyone. It's like the decoder for your secret code.

The DNS TXT record, where the public code (or public key) for DKIM is stored, typically looks like this:

```bash
v=DKIM1; k=rsa; p=NICfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDBolTXCqbxwoRBffyg2efs+Dtlc+CjxKz9grZGBaISRvN7EOZNoGDTyjbDIG8CnEK479niIL4rPAVriT54MhUZfC5UU4OFXTvOW8FWzk6++a0JzYu+FAwYnOQE9R8npKNOl2iDK/kheneVcD4IKCK7IhuWf8w4lnR6QEW3hpTsawIDAQ0B"
```

Here's the command I usually run to fetch that:
```bash
dig TXT selector1._domainkey.yourdomain.com
```

Note: Replace `selector1` with your actual selector, and `yourdomain.com` with your actual domain. This command will fetch the DNS TXT record where your public code is stored.

### DMARC (Domain-based Message Authentication, Reporting & Conformance)

DMARC: It's like the boss of SPF and DKIM. It takes the rules from SPF and DKIM and makes a big rule book. This rule book tells everyone what to do if an email from your domain doesn't follow the rules. For example, one rule could be to send a report if an email doesn't pass the checks. The DMARC Record, a place everyone can see, holds this rule book.

If an email passes the SPF and DKIM checks, the receiver then looks at the DMARC rule book to decide what to do with the email. They might follow the rule to send a report, or they might follow another rule depending on what your rule book says.

DMARC allows domain owners to declare their rules in the rule book. This rule book, stored in the DMARC Record, a DNS TXT record, specifies your DMARC policies and how receivers should handle mail that violates these rules. If both SPF and DKIM checks pass, the receiver then checks the DMARC rule book to decide what to do with the email.

The DNS TXT record for DMARC 'rule book' typically looks like this:

```bash
v=DMARC1; p=none; rua=mailto:postmaster@example.com
```

Here's the command I usually run to fetch that:
```bash
dig _dmarc.example.com TXT
```

## Real World Examples of SPF, DKIM, and DMARC Are Used

Let's look at how SPF, DKIM, and DMARC work in real-world example scenarios:

- **Mobile Apps**: Mobile apps that send emails, such as a fitness app sending workout summaries or a banking app sending transaction alerts, also use SPF, DKIM, and DMARC. When the app sends an email, the receiving server checks if the sending server's IP is in the SPF record of the sender's domain. It then uses the DKIM record to verify the email's DKIM signature. If both checks pass, the server applies the DMARC policy to decide what to do with the email. This ensures that the emails reach the user's inbox and not the spam folder, and protects the app's reputation by preventing email spoofing.

- **Email Service Providers**: Providers like Gmail, Yahoo, and Outlook use SPF, DKIM, and DMARC to authenticate incoming emails. For instance, when an email arrives, Gmail checks if the sending server's IP is in the SPF record of the sender's domain. It then uses the DKIM record to verify the email's DKIM signature. If both checks pass, Gmail applies the DMARC policy to decide what to do with the email.

- **Social Media Platforms**: Social media platforms like LinkedIn, Facebook, or Twitter that send notification emails also use SPF, DKIM, and DMARC. When a user receives a notification email, their email provider checks if the sending server's IP is in the SPF record of the social media platform's domain. It then uses the DKIM record to verify the email's DKIM signature. If both checks pass, the provider applies the DMARC policy to decide what to do with the email. This ensures that the emails reach the user's inbox and not the spam folder, and protects the social media platform's reputation by preventing email spoofing.

- **Businesses**: Businesses use SPF, DKIM, and DMARC to protect their email communication and brand reputation. For example, a business might send promotional emails to its customers. By implementing SPF, DKIM, and DMARC, the business ensures that its emails are not marked as spam and that its domain is not used for email spoofing.

- **Government Agencies**: Government agencies use SPF, DKIM, and DMARC to secure their email communication and prevent phishing attacks. For instance, a government agency might send notifications to citizens. By using SPF, DKIM, and DMARC, the agency ensures that its emails reach the citizens' inbox and that cybercriminals cannot send phishing emails that appear to come from the agency.

## Now I Know These Things, What's Next?

Now that you've learned the basics of SPF, DKIM, and DMARC, you might be thinking about using these tools to make your emails more secure. Here's a simple guide to help you get started:

1. **Identify the Email Address and Domain**: First, you need to know the email address and domain your app uses. You'll need to add SPF, DKIM, and DMARC records to this domain. A simple way to find this out is by sending an email from your app to yourself. For example, you could sign up for an account on your site and click on 'forgot password' to receive an email.

2. **Current Status**: Next, check if you already have SPF, DKIM, and DMARC records. If you do, make sure they're set up correctly. You can learn how to do this in the next section, 'Checking Your SPF, DKIM, DMARC Status'.

3. **Domain Access**: Make sure you have the rights to change the DNS records of your domain. You'll need this to add SPF, DKIM, and DMARC records. If you don't have access, you'll need to request the person who does to add these records for you.

4. **DMARC Monitoring**: Once you've set up DMARC, you'll need to keep an eye on DMARC reports to make sure everything's working as it should and fix any problems. Decide who will do this and which email address will receive the DMARC reports.

The usual order is to set up the SPF record first, then DKIM, and finally DMARC.

## Checking Your SPF, DKIM, DMARC Status

This is a straightforward with tools like MXToolbox and DMARCTester. Here's how you can use these tools:

1. **MXToolbox**:
   - Visit https://mxtoolbox.com/
   - Use the 'SPF Record Lookup', 'DKIM Record Lookup', and 'DMARC Record Lookup' tools to check the respective records for your domain.

2. **DMARCTester**:
   - Visit https://www.dmarctester.com/
   - This site offers two ways to check your email security:
     - **Send an Email**: The site generates a unique email address for you. You can then send an email from your application or mail server to this address.
     - **Paste Email Headers**: Alternatively, you can send an email from your application to your own email address, then copy the email headers and paste them into the tool.

Remember, these checks help you understand what's missing or needs improvement to enhance your email security and reputation, make sure you take note of that and take action.

Note: When using online tools, only share what's needed. Always check the site's privacy rules to keep your info safe. I'm sharing these tools because they're helpful, not because I am affiliated with them.

## FAQ's with SPF, DKIM and DMARC

1. What email address should I use for DMARC reporting?

   It's a good idea to use an email address that multiple people can check. This is often a shared mailbox. Ideally, this email address should be from the same domain that you're setting up DMARC for. If you decide to use an email address from a different domain, you'll need to add an extra step: You'll have to add a special record (called a DNS TXT record) to authorize the other domain to receive DMARC reports.

2. What's the difference between ~all, -all, ?all, and +all in an SPF record?

   These are used to tell receiving servers what to do if an email comes from a server that isn't listed in your SPF record.

   - ~all (SoftFail): This means "It's okay if the server isn't on my list, but be aware that it might not be legit." The email will still be accepted, but it might be marked as suspicious. This is often used when you're still testing your SPF record or making changes to it.

   - -all (Fail): This means "Only accept emails from servers on my list. Reject everything else." This is used when you're sure of all the servers that should be sending emails for your domain.

   - ?all (Neutral): This means "I'm not saying whether servers should be on my list or not. Treat the email as you normally would." This doesn't really give any instructions about how to handle the email, so it's not used very often.

   - +all (Pass): This means "Accept emails from all servers, even if they're not on my list." This isn't recommended because it could allow spammers to send emails that look like they're from your domain.

   The choice between these depends on how strictly you want to enforce SPF rules for your domain. It’s generally recommended to use ~all while testing or setting up your SPF record, and switch to -all once you are confident that your SPF record is correct.

3. Can I set up DMARC without SPF?

   Technically, you can, but it's not a good idea. DMARC is like a security guard for your emails. It uses two tools, SPF and DKIM, to check if an email is really from you. If an email fails both the SPF and DKIM checks, it also fails the DMARC check.

   If you set up DMARC without SPF, it's like the security guard is missing one of its tools. It can still use DKIM to check emails, but it won't be as effective.

   SPF isn't perfect and can't stop all fake emails on its own. That's why it's best to use it together with DKIM and DMARC. This gives you a more complete email security system.

4. I've looked at an email header and I see multiple SPF fails and some SPF passes. Which one should I believe?

   Think of an email header like a story. The most recent events are at the top, and the oldest events are at the bottom. So, the original sender’s information is usually towards the bottom of the header.

   If you see multiple SPF fails and a couple of SPF passes, it might feel like the story is getting confusing. But don't worry! You should trust the SPF check that's related to your domain or the domains that you trust (like your 'friends list'). The other SPF checks are for other domains that were part of the email's journey, and their pass or fail status doesn't affect your domain's SPF status.


## Wrapping Up

Just like a secret handshake, SPF, DKIM, and DMARC are the hidden heroes of email security. They're the reason your email recipients can trust messages from your domain. So, the next time you hit 'send', remember that these three musketeers are working tirelessly behind the scenes to keep your email safe.

## Contributing
Spotted a mistake or missing info in this guide? Don't be shy! Raise an issue or better yet, fork this repo and raise a PR. Your contributions help make this guide better for everyone.

## Sharing is Caring

You're welcome to share, clone, fork, or bookmark this content. All we ask is that you give credit where it's due :)

"Understanding SPF, DKIM, and DMARC: A Simple Guide" by Nicanor II Flavier, used under [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/). To view the original material, visit [https://github.com/nicanorflavier/spf-dkim-dmarc-simplified](https://github.com/nicanorflavier/spf-dkim-dmarc-simplified)

## Contact
If you have any questions or suggestions, feel free to reach out. You can find my contact details on my GitHub profile https://github.com/nicanorflavier

## References
Here are some useful resources if you want to learn more about SPF, DKIM, and DMARC:

- [SPF Project](http://www.openspf.org/)
- [DKIM.org](http://www.dkim.org/)
- [DMARC.org](https://dmarc.org/)
- [MXToolbox](https://mxtoolbox.com/)
- [DMARCTester](https://www.dmarctester.com/)



