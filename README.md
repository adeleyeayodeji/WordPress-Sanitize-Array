# WordPress-Sanitize-Array
WordPress Sanitize Array

```php

//sanitize_array
    public function sanitize_array($array)
    {
        //check if array is not empty
        if (!empty($array)) {
            //loop through array
            foreach ($array as $key => $value) {
                //check if value is array
                if (is_array($array)) {
                    //sanitize array
                    $array[$key] = is_array($value) ? $this->sanitize_array($value) : $this->sanitizeDynamic($value);
                } else {
                    //check if $array is object
                    if (is_object($array)) {
                        //sanitize object
                        $array->$key = $this->sanitizeDynamic($value);
                    } else {
                        //sanitize mixed
                        $array[$key] = $this->sanitizeDynamic($value);
                    }
                }
            }
        }
        //return array
        return $array;
    }

    //sanitize_object
    public function sanitize_object($object)
    {
        //check if object is not empty
        if (!empty($object)) {
            //loop through object
            foreach ($object as $key => $value) {
                //check if value is array
                if (is_array($value)) {
                    //sanitize array
                    $object->$key = $this->sanitize_array($value);
                } else {
                    //sanitize mixed
                    $object->$key = $this->sanitizeDynamic($value);
                }
            }
        }
        //return object
        return $object;
    }

    //dynamic sanitize
    public function sanitizeDynamic($data)
    {
        $type = gettype($data);
        switch ($type) {
            case 'array':
                return $this->sanitize_array($data);
                break;
            case 'object':
                return $this->sanitize_object($data);
                break;
            default:
                return sanitize_text_field($data);
                break;
        }
    }

```
