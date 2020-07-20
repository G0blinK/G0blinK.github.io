<?php
$callback = "callback";
if (isset($_GET['callback']))
{
    $callback = preg_replace("/[^a-z0-9.]+/i", "", $_GET['callback']);
}

$key = "";
if (isset($_GET['code']))
{
    $key = $_GET['code'];
}

if (mb_strlen($key, "UTF-8") <= 10)
{
    if ($key == "XSS_ME")
    {
        $result = "Okay! You can access <a href='#not-implemented'>the secret page</a>!";
    }
    else
    {
        $result = "Invalid code: '$key'";
    }
}
else
{
    $result = "Invalid code: too long";
}
$json = json_encode($result, JSON_HEX_TAG);

header('X-XSS-Protection: 0');
header('X-Content-Type-Options: nosniff');
header('Content-Type: text/javascript; charset=utf-8');
print "$callback($json)";