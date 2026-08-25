# rag-practice-
!pip install -q transformers sentence-transformers faiss-cpu pypdf python-docx nltk gradio pymupdf
import os
import io
import math
import gc
import json
import textwrap
import re
import random
import collections
from dataclasses import dataclass
from typing import List,Dict,Tuple
import nltk
nltk.download('punkt')
nltk.download('punkt_tab')
from nltk.tokenize import sent_tokenize
from google.colab import files
from datetime import datetime
from pypdf import PdfReader
from docx import Document as DocxDocument
from sentence_transformers import SentenceTransformer
from transformers import AutoTokenizer,AutoModelForSeq2SeqLM,pipeline
import fitz
import faiss
import numpy as np
import gradio as gr
import torch
