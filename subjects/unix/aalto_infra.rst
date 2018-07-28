
docker tag shinyeyes/perceptual-similarity:v0.1 exoti.cs.aalto.fi/graphics/perceptual-similarity:v0.1
docker push exoti.cs.aalto.fi/graphics/perceptual-similarity:v0.1




module show graphics/audio-u-net

module use /share/apps/singularity-ci/centos/modules/graphics
module load audio-u-net
singularity_wrapper exec python