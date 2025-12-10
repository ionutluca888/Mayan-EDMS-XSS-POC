# Mayan-EDMS-XSS-POC

A DOM-based Cross-Site Scripting (XSS) vulnerability was identified in the Mayan EDMS web interface.
The application reflects user-controlled data directly into a JavaScript context without proper sanitization, allowing an attacker to execute arbitrary JavaScript in the victim’s browser.

This issue can lead to account takeover, data exfiltration, privilege escalation, or full compromise of the affected user session.

The vulnerable code is located in the client-side template rendering logic responsible for handling dynamic navigation:

<script>
if (typeof partialNavigation === 'undefined') {
    document.write('<script type="text/undefined">')
    const currentLocation = '#' + window.location.pathname + window.location.search;
    const url = new URL(currentLocation, window.location.origin)
    window.location = url;
}
</script>


User-controlled data from window.location is passed into JavaScript without sanitization, which makes it possible to inject executable code.
