```R
ggplot(data = mpg) + 
        geom_jitter(mapping = aes(x = displ, y = hwy)) +
        facet_grid(drv ~ .)


                show.legend = FALSE
                     vs
        theme(legend.position = "none")




ggplot(data = mpg) +
        geom_smooth(mapping = aes(x = displ, y = hwy, group = drv))

ggplot(data = diamonds, mapping = aes(x = color, fill=cut)) +
        geom_bar()+
        facet_wrap(~ clarity)


ggplot(data = <DATA>) + 
        <GEOM_FUNCTION>(
                mapping = aes(<MAPPINGS>),
                stat = <STAT>, 
                position = <POSITION>
        ) +
        <COORDINATE_FUNCTION> +
        <FACET_FUNCTION>


library(ggplot2)
world <- map_data("world")
ggplot()+
        geom_polygon(data=world, aes(x=long, y=lat, group=group, fill=region), colour = "black")+
        theme(legend.position = "none")+
        labs(x=NULL, y=NULL)
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/index.png)
