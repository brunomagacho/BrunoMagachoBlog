---
layout: post
title:  "Codes of the Lattice Boltzmann course"
date:   2021-06-17 10:33:54 -0300
categories: jekyll update
author: "Bruno Magacho"
---

<details markdown='1'><summary>BGK2D ThinPlate Class</summary> 


```python
import numpy as np
from numpy import *
import matplotlib.pyplot as plt
from matplotlib import cm
import gc
from timeit import default_timer as timer
from evtk.hl import imageToVTK

# Flow definition

maxIter = 1000                 # Total number of time iterations.
Re = 300.0                     # Reynolds number.
nx, ny = 2000, 500             # Numer of lattice nodes.
ly = ny-1                      # Height of the domain in lattice units.
uLB     = 0.04                 # Velocity in lattice units.
nulb    = uLB*ny/Re;           # Viscoscity in lattice units.
omega = 1 / (3*nulb+0.5);      # Relaxation parameter.

# Lattice Velocities

cx = [0, 1,-1, 0, 0, 1,-1, 1,-1];
cy = [0, 0, 0, 1,-1, 1,-1,-1, 1];

# Opposite directions 

opp = [0,2,1,4,3,6,5,8,7];

# Lattice Weights

w = [4/9, 1/9, 1/9, 1/9, 1/9, 1/36, 1/36, 1/36, 1/36];

# Sound velocity in the LBM

cs = 1/np.sqrt(3);

###### Function Definitions ####################################################
def DensityVelocity(fin):
    rho = sum(fin, axis=0)
    u = zeros((2, nx, ny))
    for i in range(9):
        u[0,:,:] += cx[i] * fin[i,:,:]
        u[1,:,:] += cy[i] * fin[i,:,:]
    u = u/rho
    return rho, u

def equilibrium(rho, u):              # Equilibrium distribution function.
    usqr = 3/2 * (u[0]**2 + u[1]**2)
    feq = zeros((9,nx,ny))
    for i in range(9):
        cu = 3 * (cx[i]*u[0,:,:] + cy[i]*u[1,:,:])
        feq[i,:,:] = rho*w[i] * (1 + cu + 0.5*cu**2 - usqr)
    return feq

# Creation of a mask with 1/0 values, defining the shape of the obstacle.
def Thin_Plate_Function(x, y):
    return np.logical_and(np.logical_and(x==nx/4,y<=ny/2 + ny/10),y>=ny/2 - ny/10)

Thin_Plate = fromfunction(Thin_Plate_Function, (nx,ny))

# Initial velocity profile: almost zero, with a slight perturbation to trigger the instability.
def inivel(d, x, y):
    return (1-d) * uLB * (1 + 1e-4*sin(y/ly*2*pi))

vel = fromfunction(inivel, (2,nx,ny))

# Initialization of the populations at equilibrium with the given velocity.
fin = equilibrium(1, vel)

###### Main time loop ##########################################################

start = timer()

for time in range(maxIter):
    
    # Right wall: outflow condition.
    
    fin[[8,2,6],-1,:] = fin[[8,2,6],-2,:]

    # Compute macroscopic variables, density and velocity.
    
    rho, u = DensityVelocity(fin)

    # Left wall: inflow condition.
    
    u[:,0,:] = vel[:,0,:]
    rho[0,:] = ( sum(fin[[3,0,4],0,:], axis=0) + 2*sum(fin[[8,2,6],0,:], axis=0) )/(1-u[0,0,:])
    
    # Compute equilibrium.
    
    feq = equilibrium(rho, u)
    
    fin[[5,1,7],0,:] = feq[[5,1,7],0,:] + fin[[6,2,8],0,:] - feq[[6,2,8],0,:]
    
    # Collision step.
    fout = fin - omega * (fin - feq)

    # Full way Bounce-back
    
    for i in range(9):
        fout[i, Thin_Plate] = fin[opp[i], Thin_Plate]


    # Streaming process
    
    for i in range(9):
        fin[i,:,:] = roll(roll(fout[i,:,:], cx[i], axis=0), cy[i], axis=1)
 
    # Visualization of the velocity.

    
    if (np.logical_and(time%100==0,time/100>=0)):
        print(time/100,np.mean(rho))
        #s=str(time/100)

        #normV = np.transpose(np.sqrt(u[0]**2+u[1]**2))
        #plt.figure(figsize=(20,6.6))
        #ax = plt.gca()
        #plt.pcolor(normV, cmap = cm.rainbow, vmin=0, vmax=2*uLB)
        #plt.colorbar()
        #plt.xlabel('x')
        #plt.ylabel('y')
        #ax.xaxis.label.set_size(10)
        #plt.xticks(fontsize = 5)
        #plt.yticks(fontsize = 5)
        #ax.yaxis.label.set_size(10)
        #plt.figtext(.5,.9,'|V| - Thin Plate', fontsize=10, ha='center')
        #plt.savefig("vBGK2d"+s.zfill(6)+".png")
        #plt.clf()
        #plt.cla()
        #gc.collect()
    #plt.close('all')
    
    #Writing data for the velocity fields in binary 
    
    #if time == maxIter-1:
        #s=str(time)
        #UX = open("Vfx"+s+".dat", "wb")
        #UX.write(bytearray(u[0]))
        #UX.close()
        #UY = open("Vfy"+s+".dat", "wb")
        #UY.write(bytearray(u[1]))
        #UY.close()
        #print("<rho> ="+str(np.mean(rho)))
        
dt = timer() - start
print("The simulation finished in %f s" % dt)
```

