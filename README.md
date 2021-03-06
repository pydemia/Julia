# Julia

This contains the informations & sources of Julia which is a programming language.

## At the beginning

This is the first page.


Lorenz Attractor

```julia
# initialize the attractor
n = 1500
dt = 0.02
σ, ρ, β = 10., 28., 8/3
x, y, z = 1., 1., 1.

# initialize a 3D plot with 1 empty series
plt = path3d(1, xlim=(-25,25), ylim=(-25,25), zlim=(0,50),
                xlab = "x", ylab = "y", zlab = "z",
                title = "Lorenz Attractor", marker = 1)

# build an animated gif, saving every 10th frame
@gif for i=1:n
    dx = σ*(y - x)     ; x += dt * dx
    dy = x*(ρ - z) - y ; y += dt * dy
    dz = x*y - β*z     ; z += dt * dz
    push!(plt, x, y, z)
end every 10
```

Make some waves


```julia
using Plots
pyplot(leg=false, ticks=nothing)
x = y = linspace(-5, 5, 40)
zs = zeros(0,40)

@gif for i in linspace(0, 2π, 100)
    f(x,y) = sin(x + 10sin(i)) + cos(y)

    # create a plot with 3 subplots and a custom layout
    l = @layout [a{0.7w} b; c{0.2h}]
    p = plot(x, y, f, st = [:surface, :contourf], layout=l)

    # add a tracking line
    fixed_x = zeros(40)
    z = map(f,fixed_x,y)
    plot!(p[1], fixed_x, y, z, line = (:black, 5, 0.2))
    vline!(p[2], [0], line = (:black, 5))

    # add to and show the tracked values over time
    zs = vcat(zs, z')
    plot!(p[3], zs, alpha = 0.2, palette = cgrad(:blues).colors)
end
```

Iris Dataset

```julia
load a dataset
using RDatasets
iris = dataset("datasets", "iris");

# load the StatPlots recipes (for DataFrames) available via:
# Pkg.add("StatPlots")
using StatPlots

# Scatter plot with some custom settings
scatter(iris, :SepalLength, :SepalWidth, group=:Species,
        title = "My awesome plot",
        xlabel = "Length", ylabel = "Width",
        m=(0.5, [:cross :hex :star7], 12),
        bg=RGB(.2,.2,.2))

# save a png
png("iris")
```
