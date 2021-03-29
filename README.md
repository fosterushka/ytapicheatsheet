# ytapicheatsheet
Youtube API CheatSheet
$var - Your variable's value

HQ thumbnail = `https://i.ytimg.com/vi/`$var`/maxresdefault.jpg`


### PHP TRASH CODE
```
public function getYTid($ytURL)
    {
        $ytvIDlen = 11;

        $idStarts = strpos($ytURL, "?v=");

        if ($idStarts === false) {
            $idStarts = strpos($ytURL, "&v=");
        }
        if ($idStarts === false) {
           abort(404);
        }

        $idStarts += 3;

        return substr($ytURL, $idStarts, $ytvIDlen);
    }
```
```
    public function youtubePreviewChecker($youtubeId)
    {
        $hq = "https://i.ytimg.com/vi/{$youtubeId}/maxresdefault.jpg";

        $curl = curl_init();
        curl_setopt_array(
            $curl,
            array(
                CURLOPT_URL => $hq,
                CURLOPT_HEADER => true,
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_NOBODY => true
            )
        );

        $header = explode("\n", curl_exec($curl));
        curl_close($curl);
        if (strpos($header[0], '200') !== false) {
            return $hq;
        } else {
            return "https://img.youtube.com/vi/{$youtubeId}/hqdefault.jpg";
        }
    }
```
```
    public function youtubeValidator($youtubeLink)
    {
        $ytUrlArray = parse_url($youtubeLink);
        if ($this->section->slug === 'watch') {
            if (!empty($youtubeLink)) {
                if (isset($ytUrlArray['host'])) {
                    if ($ytUrlArray['host'] === 'www.youtube.com') {
                        $youtubeId = $this->getYTid($youtubeLink);
                        if ($youtubeId) {
                            $this->youtube_id = $youtubeId;
                            $fileSystem = new File();
                            $ytPreviewImg = $this->youtubePreviewChecker($youtubeId);
                            $this->label = $fileSystem->fromUrl($ytPreviewImg);
                        }
                    }
                } else {
                    abort(404);
                }
            }
        }
    }
 ```


# instead of trash code use this
    protected const YOUTUBE = '/(youtu\.be\/|youtube\.com\/(watch\?(.*&)?v=|(embed|v)\/))([\w-]{11})/';
    preg_match(self::YOUTUBE, $value, $m)
