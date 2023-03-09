Open redirects are commonly see in parameters such as:
- url=
- redirect=
- next=
- goto=
- redir
- redirect_to=

These parameters are commonly see in functionality such as 
- Login pages - [Example](https://hackerone.com/reports/411723)
- Logout pages
- Password reset pages
- Payment gateways - [Example](https://medium.com/@ahmadbrainworks/bug-bounty-how-i-earned-550-in-less-than-5-minutes-open-redirect-chained-with-rxss-8957979070e5)

Anytime you are redirected, you should be checking for an open redirect.
