#
# NOTE: Use the Makefiles to build this image correctly.
#

ARG BASE_IMG=<jupyter-pytorch-cuda>
FROM $BASE_IMG

USER root

# install - conda packages
# NOTE: we use mamba to speed things up
RUN export DEBIAN_FRONTEND=noninteractive \
 && apt-get update -yq \
 && apt-get install -yq --no-install-recommends \
    build-essential \
    ninja-build \
 && mamba install -y -q \
    bokeh==3.6.3 \
    cloudpickle==3.1.1 \
    dill==0.3.9 \
    ipympl==0.9.6 \
    matplotlib==3.10.0 \
    numpy==2.1.3 \
    pandas==2.2.3 \
    scikit-image==0.25.1 \
    scikit-learn==1.6.1 \
    scipy==1.15.1 \
    seaborn==0.13.2 \
    xgboost==2.1.4 \
    joblib==1.4.2 \
    optuna==3.6.1 \
    zarr==3.1.6 \
    warp-lang==1.13.0 \
 && python3 -m pip install --quiet --no-cache-dir --no-build-isolation "torch-harmonics @ git+https://github.com/NVIDIA/torch-harmonics.git@a632ca748a12bd9f74dbc1e00653317810991f74" \
 && python3 -m pip install --quiet --no-cache-dir "makani @ git+https://github.com/NVIDIA/makani.git@b38fcb2799d7dbc146fa60459f3f9823394a8bf1" \
 && python3 -m pip install --quiet --no-cache-dir "earth2studio[sfno,interp-modafno,solarradiation-afno]==0.15.0" \
 && apt-get purge -y --auto-remove \
     build-essential \
     ninja-build \
 && mamba clean -a -f -y

# install - requirements.txt
COPY --chown=${NB_USER}:${NB_GID} requirements.txt /tmp/requirements.txt
RUN python3 -m pip install -r /tmp/requirements.txt --quiet --no-cache-dir \
 && rm -f /tmp/requirements.txt