<script>
    function circumference(radius){
    return (2 * Math.pi * radius).toFixed(2);
};

    const sphere = {
        radius: r,
        result : circumference(r)
    };
    
</script>

<p>The circumference of a circle with radius {sphere.radius}  is {sphere.result} </p>